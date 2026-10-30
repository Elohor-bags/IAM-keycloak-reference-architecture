# Enterprise IAM Implementation: Keycloak Reference Architecture
**Author:** Asuelimen Joel
**Date:** 2026-10-30
**Role:** IAM Engineer (Portfolio Project)

## 1. Executive Summary
This project serves as a professional demonstration of Identity and Access Management (IAM) principles implemented using Keycloak. The goal was to design and deploy a secure, scalable authentication and authorization system that solves three distinct enterprise business problems.

### Objectives:
- **Customer Identity (CIAM):** Implementation of social identity providers and self-service onboarding to reduce user friction.
- **Workforce Identity:** Implementation of Role-Based Access Control (RBAC) for internal corporate governance.
- **Machine-to-Machine (M2M):** Securing backend service communication using the OAuth2 Client Credentials flow.

---

## 2. Environment Setup
To ensure a reproducible environment, I deployed Keycloak using Docker on a local Windows machine.

### Deployment Details:
- **Technology Stack:** Keycloak (Latest), Docker, Windows 11.
- **Configuration:** Super-admin account created for initial realm and client configuration.
- **Access Point:** `http://localhost:8080`

**Evidence:**
![Keycloak Admin Dashboard](screenshot_admin_dashboard.png)
*Figure 1: Keycloak Admin Console initialization.*

---

## 3. Scenario 1: Customer Identity & Access Management (CIAM)
The goal was to implement a user-friendly authentication system for a public-facing application.

### 3.1 Realm Isolation
I created a dedicated realm named `Customer-portal`. I avoided using the `master` realm to ensure that customer identities are logically isolated from system administration, preventing privilege escalation.

### 3.2 Self-Service Onboarding
I enabled **User Registration** and **Forgot Password** workflows. This enables a "self-service" model, allowing the business to scale its user base without increasing administrative overhead.

**Evidence:**
![Realm Login Settings](screenshot_realm_login.png)
*Figure 2: Enabling self-service registration and password recovery.*

### 3.3 Application Integration (OIDC Client)
I configured a client named `customer-web-app` using the **OpenID Connect (OIDC)** protocol and the **Authorization Code Flow**. This ensures that the application never handles user credentials directly, significantly reducing the risk of credential theft.

**Evidence:**
![Client Configuration](screenshot_client_config.png)
*Figure 3: OIDC Client configuration for the web application.*

### 3.4 Identity Brokering (Social Login)
I implemented **Identity Brokering** using GitHub as an external Identity Provider (IdP). This leverages the security infrastructure of a trusted provider and reduces "password fatigue" for the end-user.

**Evidence:**
![Social Login Page](screenshot_social_login.png)
*Figure 4: The customized login screen showing the GitHub identity broker option.*

---

## 4. Scenario 2: Workforce Identity & RBAC
In this scenario, I focused on internal corporate governance where identity is strictly managed.

### 4.1 Corporate Realm Governance
I established a separate realm, `Corporate-Internal`, and disabled self-registration. This ensures that account creation is a privileged operation tied to HR onboarding.

### 4.2 Implementation of RBAC (Role-Based Access Control)
I implemented an RBAC model to manage permissions at scale. Rather than assigning permissions to users individually, I assigned permissions to roles.

**Role Hierarchy:**
- `employee`: Base role granting access to general company resources.
- `manager`: Elevated role granting access to sensitive reporting tools.

**Evidence:**
![Staff User Roles](screenshot_staff_roles.png)
*Figure 5: Least-privilege access for staff users.*

![Manager User Roles](screenshot_manager_roles.png)
*Figure 6: Elevated access for management users.*

---

## 5. Scenario 3: Machine-to-Machine (M2M) Security
I implemented a secure communication channel for backend services without human intervention.

### 5.1 Client Credentials Flow
I utilized the **OAuth2 Client Credentials Flow** via a service account for a client named `backend-service-app`. I explicitly disabled the "Standard Flow" to eliminate unnecessary attack vectors since no browser is involved.

### 5.2 Service Account Authorization
Adhering to the **Principle of Least Privilege (PoLP)**, I assigned only the specific role required for the service's function (`manager`), avoiding "role bloat" and reducing the blast radius in the event of a credential leak.

**Evidence:**
![M2M Service Account Roles](screenshot_m2m_roles.png)
*Figure 7: Service account role assignment for machine-to-machine authentication.*

---

## 6. Technical Deep Dives & Analysis

### 6.1 The "Shadow User" Concept
During the implementation of Identity Brokering (GitHub), I analyzed the Just-In-Time (JIT) provisioning process. When a user first authenticates via GitHub, Keycloak creates a "shadow user" locally. This allows the system to:
1. Maintain a unique local anchor (Subject ID).
2. Assign internal roles and attributes that are not present in the external IdP.
3. Manage the user's lifecycle independently of the external provider.

### 6.2 Security Analysis: Flow Comparison
| Use Case | Recommended Flow | Why? |
| :--- | :--- | :--- |
| **Web App (Users)** | Authorization Code + PKCE | Most secure for browsers; credentials never touch the app. |
| **Backend (M2M)** | Client Credentials | No human interaction; based on trusted Client Secret. |
| **Admin (Control)** | Master Realm $\rightarrow$ Isolated | Prevents privilege escalation from user realms. |

## 7. Conclusion
This project demonstrates a comprehensive understanding of the IAM lifecycle—from public-facing CIAM and corporate RBAC to secure M2M communication. By leveraging Keycloak, I have built a reference architecture that balances user experience with stringent security governance.
