# Enterprise IAM Reference Architecture with Keycloak 🛡️

[![IAM Engineer Portfolio](https://img.shields.io/badge/Role-IAM%20Engineer-blue)](https://github.com)
[![Tech Stack](https://img.shields.io/badge/Stack-Keycloak%20%7C%20Docker%20%7C%20OIDC-green)](https://github.com)

This project is a professional implementation of Identity and Access Management (IAM) patterns designed to solve real-world enterprise challenges. It demonstrates the ability to design secure authentication and authorization systems for customers, employees, and machine-to-machine communication.

## 🚀 Project Overview

The architecture focuses on three core pillars of modern identity:

### 1. Customer Identity (CIAM)
- **Goal:** Reduce onboarding friction while maintaining security.
- **Key Features:** Self-service registration, Password recovery, and Identity Brokering (GitHub Social Login).
- **Protocol:** OpenID Connect (OIDC) - Authorization Code Flow.

### 2. Workforce Identity (Corporate)
- **Goal:** Strict governance and centralized control over internal access.
- **Key Features:** Disabled public registration, Role-Based Access Control (RBAC), and strict administrative onboarding.
- **Authorization:** Role Hierarchy (`Employee` $\rightarrow$ `Manager`).

### 3. Machine-to-Machine (M2M) Security
- **Goal:** Secure service-to-service communication without human intervention.
- **Key Features:** Client Credentials Flow, Service Account Provisioning, and Least Privilege role assignment.
- **Protocol:** OAuth2.

## 🛠️ Technical Stack
- **Identity Provider:** Keycloak
- **Containerization:** Docker & Docker Compose
- **Protocols:** OIDC, OAuth2, SAML
- **Infrastructure:** Infrastructure as Code (IaC) via Docker Compose

## 📁 Repository Structure
- `/docs`: Full engineering documentation and architectural analysis.
- `/screenshots`: Visual evidence of implementation and configuration.
- `docker-compose.yml`: The environment blueprint for rapid deployment.

## 📖 Full Documentation
For a deep dive into the engineering decisions, security analysis, and "shadow user" implementation, please refer to the full project report:
👉 **[View Full Project Documentation](./docs/IAM_Portfolio_Project.md)**

---
*Designed as a professional reference for IAM Engineering roles.*
