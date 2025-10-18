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
An identity requesting access—can be a user, group, service principal, or managed identity.

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

📌 *Coming soon: Visual walkthroughs of JIT & PIM flows in Azure Portal*







