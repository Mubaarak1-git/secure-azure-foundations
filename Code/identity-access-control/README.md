# Identity & Access Control  
## 🔐 Azure Role-Based Access Control (RBAC)

Azure RBAC is a fine-grained authorization system built on Azure Resource Manager. It helps organizations manage **who** can access resources, **what** actions they can perform, and **where** those actions apply.

---

### 🎯 What You Can Do with Azure RBAC

- Assign VM management to one user, and network management to another  
- Grant a DBA group access to SQL databases  
- Allow scoped access to a resource group for users or applications  
- Enforce least privilege across subscriptions and services

---

### 🧩 Core Concepts of RBAC

#### 👤 Security Principal  
An identity requesting access can be a user, group, service principal, or managed identity.

#### 📜 Role Definition  
A set of permissions (Actions, NotActions, DataActions). Examples:  
- **Owner** – full access  
- **Reader** – view-only  
- **Virtual Machine Contributor** – manage VMs

Custom roles can be created if built-in roles don’t meet your needs.

#### 📦 Scope  
Defines where access applies:  
- Management Group  
- Subscription  
- Resource Group  
- Resource

Scopes follow a parent-child hierarchy and can be used to narrow access precisely.

---

### 🔗 Role Assignments

A **role assignment** connects a security principal to a role definition at a specific scope.

- Access is **granted** by creating a role assignment  
- Access is **revoked** by removing it

Assignments can be made via:  
Azure Portal, CLI, PowerShell, SDKs, or REST APIs.

---

### 👥 Groups and Transitive Access

RBAC supports **transitive group membership**:  
If a user is in a group that’s nested inside another group with a role assignment, the user inherits those permissions.

---

### ➕ Multiple Role Assignments

Azure RBAC is **additive**:  
Effective permissions = sum of all assigned roles.

Example:  
- Contributor at subscription level  
- Reader at resource group level  
→ Contributor permissions apply across both.

---

### 🔍 Access Evaluation Logic

Azure Resource Manager follows these steps to determine access:

1. User/service principal requests a token  
2. Token includes group memberships  
3. REST API call is made with token  
4. Azure retrieves role + deny assignments  
5. Deny assignment blocks access if present  
6. Role assignments are filtered by scope  
7. Actions and NotActions are evaluated  
8. DataActions and NotDataActions are evaluated  
9. Conditions (if any) are checked  
10. Access is granted or denied

**Formula**:  
`Actions - NotActions = Effective management permissions`  
`DataActions - NotDataActions = Effective data permissions`

---

### 🌍 Where Is RBAC Data Stored?

RBAC data (role definitions, assignments, deny assignments) is stored **globally**.

- Ensures access regardless of resource region  
- Replicated across Azure regions for speed and resilience  
- Enforced by Azure Resource Manager’s global endpoint

**Example**:  
A VM created in East Asia can be accessed by a team member in the U.S. because RBAC data is globally replicated.

---

### 💸 License Requirements

Azure RBAC is **free** and included in your Azure subscription.

---
## 🔐 Just-in-Time (JIT) VM Access – Defender for Cloud
JIT access is a dynamic security control in Microsoft Defender for Cloud (Plan 2) that reduces attack surfaces by locking down inbound traffic to management ports (like RDP and SSH) and granting access only when needed.

### 🎯 What You Can Do with JIT Access
- Block always-open RDP/SSH ports on Azure and AWS VMs

- Allow temporary access for specific users, IPs, and timeframes

- Enforce least privilege at the network level

- Respond to Defender for Cloud recommendations for unhealthy VMs
  
### 🧩 Core Concepts of JIT Access
#### 🔒 Target Ports
Focuses on high-risk management ports:

- RDP (3389)

- SSH (22)

- Others as defined by your workload

#### 🧠 Access Logic
JIT access follows a request-approve-expire model:

1. Request: User initiates access to a locked port

2. Approve: Defender for Cloud validates and opens ports temporarily

3. Expire: Access auto-revokes after the defined time window

### 📦 Scope of Enforcement
JIT rules apply at the network layer, enforced via:

- Network Security Groups (NSGs)

- Azure Firewall rules
  
If existing rules already govern the port, they take precedence. Otherwise, JIT rules are prioritized.

### 🔗 JIT Rule Assignments
JIT access is granted by creating temporary inbound rules:

- Scoped to specific ports, IP ranges, and durations

- Managed via Azure Portal, CLI, PowerShell, or REST API

- Automatically removed after expiration

### 🧭 VM Categorization Logic
Defender for Cloud scans supported VMs and:

- Flags those without JIT as Unhealthy

- Recommends enabling JIT for better posture

- Supports both Azure and AWS environments

### 🧮 Access Evaluation Formula
JIT access = Requested Port + Approved IP + Time Window → Access granted Outside this formula → Access denied

### 🌍 Where Is JIT Data Stored?
JIT configurations are stored and enforced via Azure Resource Manager:

- Globally replicated for resilience

- Integrated with Defender for Cloud’s recommendation engine

### 💸 License Requirements
JIT access is available with:

- **Microsoft Defender for Servers Plan 2**

- Requires Defender for Cloud integration
  
📌 *Coming soon: Visual walkthroughs of PIM flows in Azure environment*







