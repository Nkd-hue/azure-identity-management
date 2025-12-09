#  The Merchandise Vault Hybrid Identity and Access Management Solution

> A comprehensive enterprise-grade Identity and Access Management (IAM) solution built on Microsoft Entra ID (Azure AD), implementing Zero Trust security principles with RBAC, Conditional Access, MFA, and Privileged Identity Management.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [How I Built This](#️-how-i-built-this)
- [Screenshots](#-screenshots)
- [RBAC Matrix](#-rbac-matrix)
- [Conditional Access Policies](#-conditional-access-policies)
- [Security Considerations](#-security-considerations)


---

## 🎯 Overview

This project demonstrates the implementation of a comprehensive Hybrid Identity and Access Management solution for **The Merchandise Vault**, showcasing best practices in enterprise security using Microsoft Azure cloud services.

### Business Objectives

| Objective | Solution |
|-----------|----------|
| Centralized Identity Management | Microsoft Entra ID with custom domain |
| Least Privilege Access | Role-Based Access Control (RBAC) |
| Strong Authentication | Multi-Factor Authentication (MFA) |
| Context-Aware Security | Conditional Access Policies |
| Just-in-Time Admin Access | Privileged Identity Management (PIM) |
| User Self-Service | Self-Service Password Reset (SSPR) |
| Delegated Administration | Administrative Units |

### Skills Demonstrated

- ✅ Microsoft Entra ID (Azure Active Directory)
- ✅ Role-Based Access Control (RBAC)
- ✅ Conditional Access Policies
- ✅ Multi-Factor Authentication (MFA)
- ✅ Privileged Identity Management (PIM)
- ✅ Self-Service Password Reset (SSPR)
- ✅ Administrative Units
- ✅ Security Groups & Dynamic Membership
- ✅ Custom Role Definitions
- ✅ Zero Trust Security Model

---

## 🏗 Architecture

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                  THE MERCHANDISE VAULT IDENTITY MANAGEMENT ARCHITECTURE                     │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                        MICROSOFT ENTRA ID                                │   │
│   │                                                                          │   │
│   │   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │   │
│   │   │   Custom    │  │Administrative│  │   Security  │  │  Privileged │   │   │
│   │   │   Domain    │  │    Units    │  │   Groups    │  │   Identity  │   │   │
│   │   │             │  │             │  │             │  │  Management │   │   │
│   │   │ techcorp.com│  │  US-Unit    │  │  IT-Admins  │  │             │   │   │
│   │   │             │  │  EU-Unit    │  │  Developers │  │  Just-in-   │   │   │
│   │   │             │  │  APAC-Unit  │  │  Finance    │  │  Time Access│   │   │
│   │   └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘   │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                        ACCESS CONTROL LAYER                              │   │
│   │                                                                          │   │
│   │   ┌───────────────────┐  ┌───────────────────┐  ┌─────────────────┐    │   │
│   │   │  Conditional      │  │  Multi-Factor     │  │  Self-Service   │    │   │
│   │   │  Access Policies  │  │  Authentication   │  │  Password Reset │    │   │
│   │   │                   │  │                   │  │                 │    │   │
│   │   │ • Location-based  │  │ • Authenticator   │  │ • Email verify  │    │   │
│   │   │ • Device-based    │  │ • SMS/Phone       │  │ • Phone verify  │    │   │
│   │   │ • Risk-based      │  │ • FIDO2 keys      │  │ • Security Q's  │    │   │
│   │   └───────────────────┘  └───────────────────┘  └─────────────────┘    │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                    ROLE-BASED ACCESS CONTROL (RBAC)                      │   │
│   │                                                                          │   │
│   │    Subscription Level        Resource Group Level      Resource Level    │   │
│   │   ┌─────────────────┐       ┌─────────────────┐      ┌──────────────┐   │   │
│   │   │ • Owner         │       │ • Contributor   │      │ • Reader     │   │   │
│   │   │ • Contributor   │ ────▶ │ • Reader        │ ───▶ │ • Specific   │   │   │
│   │   │ • Reader        │       │ • Custom Roles  │      │   Operators  │   │   │
│   │   └─────────────────┘       └─────────────────┘      └──────────────┘   │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
│                                        │                                         │
│                                        ▼                                         │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │                           AZURE RESOURCES                                │   │
│   │                                                                          │   │
│   │   ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │   │
│   │   │  Production │  │ Development │  │   Finance   │  │   Shared    │   │   │
│   │   │  Resources  │  │  Resources  │  │  Resources  │  │  Services   │   │   │
│   │   │  RG-Prod-*  │  │  RG-Dev-*   │  │ RG-Finance-*│  │ RG-Shared-* │   │   │
│   │   └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘   │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Component Interaction Flow

```
┌──────────┐     ┌─────────────┐     ┌──────────────────┐     ┌────────────────┐
│   User   │────▶│  Entra ID   │────▶│  Conditional     │────▶│  Azure         │
│          │     │  Auth       │     │  Access Check    │     │  Resources     │
└──────────┘     └─────────────┘     └──────────────────┘     └────────────────┘
                       │                      │
                       ▼                      ▼
                 ┌─────────────┐      ┌──────────────────┐
                 │  MFA        │      │  RBAC            │
                 │  Challenge  │      │  Evaluation      │
                 └─────────────┘      └──────────────────┘
```

---

## ✨ Features

### 🔑 Identity Management

- **Custom Domain Integration** - Professional user identities with `@techcorp.com`
- **User Lifecycle Management** - Automated provisioning and deprovisioning workflows
- **Dynamic Groups** - Automatic group membership based on user attributes
- **Administrative Units** - Regional delegation for US, EU, and APAC

### 🛡️ Access Control

- **RBAC at Multiple Scopes** - Subscription, Resource Group, and Resource level permissions
- **Custom Role Definitions** - Tailored roles like "VM Operator" for specific use cases
- **Separation of Duties** - Finance resources isolated from IT direct access
- **Least Privilege Principle** - Users receive minimum necessary permissions

### 🔐 Authentication & Security

- **Multi-Factor Authentication** - Microsoft Authenticator, FIDO2 keys, SMS backup
- **Conditional Access** - 7 policies covering location, device, app, and risk conditions
- **Legacy Auth Blocking** - Protection against password spray attacks
- **Risk-Based Access** - Dynamic policy enforcement based on sign-in risk

### ⚡ Privileged Access

- **Just-in-Time Access** - Time-limited privilege activation via PIM
- **Approval Workflows** - Manager approval for sensitive role activations
- **Access Reviews** - Quarterly review of privileged role assignments
- **Break Glass Account** - Emergency access with monitoring and alerts

### 🔄 Self-Service Capabilities

- **Password Reset** - Users can reset passwords without IT intervention
- **MFA Registration** - Self-service enrollment with registration campaigns
- **Profile Updates** - Users can manage their own profile information

---

## 📦 Prerequisites

| Requirement | Details |
|-------------|---------|
| **Azure Subscription** | Active subscription with billing enabled |
| **Microsoft Entra ID P2** | Required for PIM, Conditional Access, Access Reviews |
| **Global Administrator** | Initial setup requires Global Admin role |
| **Custom Domain** | Domain you own for professional identities (optional) |
| **Test Devices** | Multiple devices/browsers for policy validation |

### Licensing Requirements

| Feature | License Required |
|---------|-----------------|
| Basic user management | Free |
| Self-Service Password Reset | P1 or P2 |
| Conditional Access | P1 or P2 |
| Dynamic Groups | P1 or P2 |
| Privileged Identity Management | P2 |
| Risk-Based Conditional Access | P2 |
| Access Reviews | P2 |


---

### 🛠️ How I Built This

### Phase 1: Foundation Setup
Started by configuring the Microsoft Entra ID tenant with a custom domain (`techcorp.com`) and setting baseline tenant security settings. Disabled user consent for applications and restricted guest access to maintain control over the directory.

### Phase 2: Identity Structure
Created a hierarchical group structure using security groups for each department (IT, Finance, Operations). Implemented **dynamic groups** with membership rules based on user attributes — for example, the `SG-All-Employees` group automatically includes all enabled member accounts.

### Phase 3: Administrative Delegation
Set up **Administrative Units** for regional delegation (US, EU, APAC), allowing regional IT admins to manage only users within their geography. This follows the principle of least privilege for administrative access.

### Phase 4: RBAC Design
Designed a tiered RBAC model:
- **Subscription level**: IT Administrators get Contributor; all employees get Reader
- **Resource Group level**: Granular permissions (e.g., Developers get Contributor only in RG-Development)
- **Custom roles**: Created a "VM Operator" role for Operations team with only start/stop/restart permissions

Key design decision: IT gets **Reader** (not Contributor) on Finance resources — enforcing separation of duties.

### Phase 5: Authentication Hardening
Configured MFA with **Microsoft Authenticator** as the primary method and enabled **number matching** to prevent MFA fatigue attacks. Disabled SMS for administrators due to SIM-swapping risks.

### Phase 6: Conditional Access
Built 7 layered policies following a defense-in-depth approach:

| Layer | Policy | Purpose |
|-------|--------|---------|
| Baseline | CA001 | MFA for all users |
| Geographic | CA002 | Block high-risk countries |
| Device | CA003 | Require compliant devices for sensitive apps |
| Network | CA004 | MFA outside corporate network |
| Protocol | CA005 | Block legacy authentication |
| Privileged | CA006 | Phishing-resistant MFA for admins |
| Session | CA007 | Time-limited sessions for sensitive apps |

Tested all policies in **Report-only mode** first using the What If tool before enforcement.

### Phase 7: Privileged Access Management
Implemented **Privileged Identity Management (PIM)** to eliminate standing admin access:
- All admin roles require just-in-time activation
- Maximum activation: 8 hours
- Global Admin requires approval + justification
- Created a monitored **break glass account** for emergencies

### Phase 8: Self-Service Enablement
Enabled **Self-Service Password Reset (SSPR)** requiring 2 verification methods, reducing help desk dependency while maintaining security. Configured notifications to alert admins when passwords are reset.

### Lessons Learned

| Challenge | Solution |
|-----------|----------|
| Policy conflicts blocking legitimate users | Used What If tool extensively before enabling policies |
| Dynamic group delays | Allowed 24 hours for membership processing before testing |
| PIM activation confusion | Created user documentation with step-by-step screenshots |
| Legacy app compatibility | Documented exceptions with compensating controls |


---

## 📸 Screenshots

### Phase 1: Microsoft Entra ID Setup

#### Custom Domain Configuration
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 12 11 30 AM" src="https://github.com/user-attachments/assets/82d629ed-1ff4-4622-b252-6518aefe4f7b" />

Description: Custom domain verification in Microsoft Entra admin center

#### Tenant Overview
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 12 19 29 AM" src="https://github.com/user-attachments/assets/2603ed12-7311-49e9-9998-ff73cf745839" />

Description: TechCorp tenant overview showing license and configuration status

---

### Phase 2: Users and Groups

#### Security Groups
<img width="1710" height="1107" alt="Screenshot 2025-12-03 at 5 12 45 PM" src="https://github.com/user-attachments/assets/ff351996-1cba-4c8f-984c-ea98a11c6954" />

Description: Security groups created for RBAC assignments

#### Dynamic Group Configuration
<img width="1710" height="1107" alt="Screenshot 2025-12-03 at 5 00 39 PM" src="https://github.com/user-attachments/assets/e96b36ab-1656-4685-b2f7-658f9d96aba3" />

Description: Dynamic membership rules for All-Employees group

#### Test Users
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 2 03 52 PM" src="https://github.com/user-attachments/assets/7a5e12b2-0e36-43c8-94b4-e21b678f095e" />

Description: Test users created across different departments

### Assign license to users and groups

<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 2 39 10 PM" src="https://github.com/user-attachments/assets/32278117-74c4-4d65-9c9f-3a9a48d1f90c" />
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 2 39 53 PM" src="https://github.com/user-attachments/assets/c48513c4-6f98-4ba2-82e2-f8a20ef2c437" />

Description: Users and groups assigned P2 license on Microsoft 365

---

### Phase 3: Administrative Units

#### Administrative Units Overview
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 8 47 08 PM" src="https://github.com/user-attachments/assets/5add8c15-c403-4ae8-9931-3f98d901fb7c" />

Description: Administrative Units across continents 

#### Scoped Role Assignment
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 8 43 45 PM" src="https://github.com/user-attachments/assets/0abd57c7-f8ea-4da6-bc0a-9f0f462afe96" />

Description: User Administrator role scoped to US administrative unit

<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 8 56 02 PM" src="https://github.com/user-attachments/assets/42cb6c56-846b-4819-b3ee-f94dc40d2175" />

Description: Restricted Adminstrative Unit

<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 8 56 38 PM" src="https://github.com/user-attachments/assets/38e0fed9-ab75-4b9f-aeee-273c6eb16e6e" />

Description: Restricted Administrative unit assigned to the Chief Technology Officer

---

### Phase 4: RBAC Implementation

#### Subscription-Level RBAC
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 3 11 10 PM" src="https://github.com/user-attachments/assets/01e95f09-c5ff-4967-9fa3-5534da6abccc" />

Description: Role assignments at the subscription level

#### Resource Group Permissions
<img width="1710" height="1107" alt="Screenshot 2025-12-05 at 9 18 02 PM" src="https://github.com/user-attachments/assets/048ce545-81d1-4585-a381-86d174720a5e" />

Description: Resource Groups


<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 2 52 16 PM" src="https://github.com/user-attachments/assets/2460bb64-b013-4228-96be-535d06c69831" />

Description: Granular permissions on RG-Finance resource group

#### Custom Role Definition
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 2 26 55 PM" src="https://github.com/user-attachments/assets/8b2f102a-31ef-4123-99db-c6f68b2da4cf" />

Description: Custom role created

<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 4 17 22 PM" src="https://github.com/user-attachments/assets/00854ce5-bafe-4a49-9636-22487bb76eea" />

Description: Custom VM Operator role for operations team

---

### Phase 5: Multi-Factor Authentication

#### Authentication Methods
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 8 21 57 PM" src="https://github.com/user-attachments/assets/aa582d9e-6e0e-4ff4-872f-e223e8b8b22b" />
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 8 27 03 PM" src="https://github.com/user-attachments/assets/130bf02e-2cec-4f52-91d7-1374ceb6ae5f" />

Description: MFA methods configured for the organization

<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 8 28 30 PM" src="https://github.com/user-attachments/assets/258b0d4e-785c-4b59-8961-928f1c359f79" />

Description:FIDO2 security keys configuration

<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 8 30 40 PM" src="https://github.com/user-attachments/assets/72c0b579-1e7b-4c5c-ae4f-e1b9cc506b2a" />

Description: SMS configuration

#### Registration Campaign
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 8 40 49 PM" src="https://github.com/user-attachments/assets/5827dfa6-b771-41e9-9215-2da75a9bd2a6" />

Description: MFA registration nudge configuration

#### Number Matching
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 8 27 03 PM" src="https://github.com/user-attachments/assets/79c72120-de65-44fb-8ab0-5383bbe414a9" />

Description: Number matching enabled to prevent MFA fatigue attacks

---

### Phase 6: Conditional Access

#### Policy Overview
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 9 48 33 PM" src="https://github.com/user-attachments/assets/fe5ce278-e720-4ebb-ae33-0c5c1af1c4ca" />

Description: All conditional access policies deployed

#### MFA Policy Configuration
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 9 51 10 PM" src="https://github.com/user-attachments/assets/669efc03-a562-4f66-b70a-0d682079ed1c" />
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 9 51 14 PM" src="https://github.com/user-attachments/assets/d0a65177-1e66-43bc-9eda-43584b5addf2" />

Description:  Require MFA for all users configuration

#### Block Legacy Authentication
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 9 54 36 PM" src="https://github.com/user-attachments/assets/59239b6c-d045-4822-a15b-537490d704d7" />

Description: Block legacy authentication protocols

#### Location-Based Policy
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 9 57 33 PM" src="https://github.com/user-attachments/assets/35f2396e-6a16-4c6a-a8b8-37d04eb86a19" />

Description: Named locations and geographic restrictions

#### What If Analysis
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 10 07 39 PM" src="https://github.com/user-attachments/assets/b7b2dc54-83b1-4941-a19f-4456d8ae4581" />

Description: Testing policy evaluation with What If tool

---

### Phase 7: Privileged Identity Management

#### PIM Dashboard
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 10 45 03 PM" src="https://github.com/user-attachments/assets/2438192b-e55b-498c-8828-19455ffae309" />

Description: Privileged Identity Management overview

#### Eligible Role Assignments
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 11 27 44 PM" src="https://github.com/user-attachments/assets/c1dac86b-ea6d-46a1-b723-f2fc36720190" />

Description: Users eligible to activate privileged roles

#### Role Settings
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 11 29 54 PM" src="https://github.com/user-attachments/assets/cda4b051-16cf-4d69-a0e6-5285c94ec742" />
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 11 30 03 PM" src="https://github.com/user-attachments/assets/363c6f1c-a110-4cb7-80cb-a06de767d1ad" />

Description: Global Administrator role activation settings

---

### Phase 8: Self-Service Password Reset

#### SSPR Configuration
<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 11 33 33 PM" src="https://github.com/user-attachments/assets/e61486ae-6ae9-4820-9a09-335ef46aaea1" />

<img width="1710" height="1107" alt="Screenshot 2025-12-08 at 11 35 05 PM" src="https://github.com/user-attachments/assets/8d9c8c62-1fff-4983-afc0-398c590b7ddb" />

Description: Self-service password reset settings

#### Authentication Methods for SSPR
<img width="1710" height="1073" alt="Screenshot 2025-12-08 at 11 35 38 PM" src="https://github.com/user-attachments/assets/0ceeada9-1dec-4278-99ca-0fbd0ab8a753" />

Description: Methods available for password reset verification


---

## 📊 RBAC Matrix

### Subscription Level

| Group | Role | Inheritance |
|-------|------|-------------|
| SG-IT-Administrators | Contributor | Flows to all RGs |
| SG-All-Employees | Reader | Flows to all RGs |

### Resource Group Level

| Group | RG-Production | RG-Development | RG-Finance | RG-SharedServices |
|-------|---------------|----------------|------------|-------------------|
| **SG-IT-Administrators** | Contributor *(inherited)* | Contributor *(inherited)* | Reader *(explicit)* | Contributor *(inherited)* |
| **SG-IT-Developers** | Reader | Contributor | — | Reader *(inherited)* |
| **SG-Finance-Admins** | — | — | Contributor | Reader *(inherited)* |
| **SG-Finance-Team** | — | — | Reader | Reader *(inherited)* |
| **SG-Operations-Team** | VM Operator *(custom)* | Reader *(inherited)* | — | Reader *(inherited)* |
| **SG-Executives** | Reader *(inherited)* | Reader *(inherited)* | Reader *(inherited)* | Reader *(inherited)* |

### Custom Roles

| Role Name | Description | Assignable Scopes |
|-----------|-------------|-------------------|
| VM Operator | Start, stop, restart VMs only | RG-Production |

---

## 🔒 Conditional Access Policies

| Policy ID | Name | Target | Conditions | Controls |
|-----------|------|--------|------------|----------|
| CA001 | Require MFA - All Users | All users (excl. admins) | All apps | Require MFA |
| CA002 | Block High-Risk Countries | All users | High-risk locations | Block access |
| CA003 | Require Compliant Device | Finance team | Azure Management | Compliant device |
| CA004 | MFA Outside Corp Network | All users | External locations | Require MFA |
| CA005 | Block Legacy Auth | All users | Legacy clients | Block access |
| CA006 | MFA for Administrators | All admin roles | All apps | Phishing-resistant MFA |
| CA007 | Session Controls | All users | Sensitive apps | 1-hour sign-in frequency |

---

## 🛡️ Security Considerations

### Zero Trust Principles Applied

| Principle | Implementation |
|-----------|---------------|
| **Verify explicitly** | MFA, Conditional Access, device compliance |
| **Use least privilege** | RBAC, PIM, just-in-time access |
| **Assume breach** | Session controls, continuous access evaluation |

### Security Best Practices

- ✅ No permanent Global Administrators (except break glass)
- ✅ All admin roles require PIM activation
- ✅ MFA enforced for all users
- ✅ Legacy authentication blocked
- ✅ High-risk countries blocked
- ✅ Break glass account monitored with alerts
- ✅ Security groups used for all RBAC assignments
- ✅ Separation of duties between IT and Finance

### Emergency Access

A break glass account is maintained for emergency scenarios:
- Excluded from all Conditional Access policies
- No MFA (intentional - MFA could be the failure point)
- Monitored with alerts on any sign-in
- Credentials stored in secure vault with dual-control access

---


## 🧪 Testing & Validation

### Test Cases Executed

| Test ID | Description | Expected Result | Status |
|---------|-------------|-----------------|--------|
| TC01 | User sign-in with correct password | Success | ✅ Pass |
| TC02 | MFA prompt outside corporate network | MFA shown | ✅ Pass |
| TC03 | No MFA from trusted IP | No MFA | ✅ Pass |
| TC04 | Legacy auth (IMAP) blocked | Blocked | ✅ Pass |
| TC05 | Sign-in from high-risk country | Blocked | ✅ Pass |
| TC06 | Developer create VM in RG-Development | Success | ✅ Pass |
| TC07 | Developer create VM in RG-Production | Denied | ✅ Pass |
| TC08 | Finance view Finance resources | Success | ✅ Pass |
| TC09 | PIM role activation | Success | ✅ Pass |
| TC10 | SSPR password reset | Success | ✅ Pass |

---


## 👥 Author

Nadia Kroduah
- linkedIn: www.linkedin.com/in/nadia-kroduah
---



