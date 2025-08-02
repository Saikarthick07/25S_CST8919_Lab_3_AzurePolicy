# Cloud Governance Gone Rogue – Azure Policy Lab

### Course: CST8919 – DevOps Security and Compliance  
### Lab – Enforcing Organizational Policies in the Cloud  

### YouTube Demo : https://youtu.be/lrFl93tOyiQ 
---

## Scenario

Welcome to **MapleTech Solutions**, a rapidly growing Canadian cloud-native company.

You’ve just been hired as a **Cloud Security Engineer**, and during your onboarding, you're told:

> “Developers are deploying resources across the globe, exposing public IPs, and skipping tags. We need order. Lock things down and enforce our cloud compliance policies — starting today.”

Your mission is to bring **governance, security, and compliance** to Azure using **Azure Policy**.

---

## Lab Objectives

- Create and assign **Azure Policies** and a **Policy Initiative**
- Enforce:
  - Region restriction to **Canada Central**
  - Mandatory tagging (`ProjectName`)
  - No public IP addresses
- Test enforcement with sample deployments
---

# Lab: Enforcing Organizational Policies in Azure

## 📌 Scenario
Welcome to **MapleTech Solutions**, a rapidly growing Canadian cloud-native company.  
Developers have been deploying resources across multiple regions, exposing public IPs, and skipping required tags.  
As the new **Cloud Security Engineer**, your task is to bring governance, security, and compliance using **Azure Policy**.

This lab guides you through creating and enforcing policies to restrict developer actions and ensure compliance.

---

## 🎯 Lab Objectives
- Create and assign **Azure Policies** and a **Policy Initiative**
- Enforce the following rules:
  1. **Region restriction** → Canada Central only
  2. **Mandatory tagging** → `ProjectName`
  3. **No Public IP addresses**
- Assign the initiative to a resource group
- Simulate developer activity to test enforcement

---

## 🛠️ Part 1: Define the Guardrails (Custom Policies)

### Policy 1: Region Lockdown
- **Name:** `Only-CanadaCentral`  
- **Effect:** Deny  
- **Description:** Denies any resource that is not being deployed in the Canada Central region.  

**Policy Rule**

```json
{
  "mode": "All",
  "policyRule": {
    "if": {
      "field": "location",
      "notEquals": "canadacentral"
    },
    "then": {
      "effect": "deny"
    }
  }
}
```

### Policy 2: Mandatory Tagging

- Name: Require-ProjectName-Tag
- Effect: Deny
- Description: Requires all resources to include a ProjectName tag.

Policy Rule
```
{
  "mode": "Indexed",
  "parameters": {},
  "policyRule": {
    "if": {
      "field": "tags['ProjectName']",
      "exists": "false"
    },
    "then": {
      "effect": "deny"
    }
  }
}
```

### Policy 3: Block Public IPs

- Name: Deny-Public-IP
- Effect: Deny
- Description: Prevents creation of Public IP addresses.
  
Policy Rule
```
{
  "mode": "Indexed",
  "policyRule": {
    "if": {
      "field": "type",
      "equals": "Microsoft.Network/publicIPAddresses"
    },
    "then": {
      "effect": "deny"
    }
  }
}
```

### 🗂️ Part 2: Group Policies into an Initiative

- Name: MapleTech Secure Foundation
- Category: Security (or Compliance)
- Includes:
```
Only-CanadaCentral
Require-ProjectName-Tag
Deny-Public-IP
```

### 🔒 Part 3: Assign the Initiative

- Scope → Assign to a resource group (e.g., DevRG).
- Initiative definition → MapleTech Secure Foundation.
- Enforcement mode → Enabled.
- Save assignment.
  
### 🧪 Part 4: Simulate Developer Activity

- Test Case	Action	Expected Outcome
- 1	Deploy VM in East US	❌ Denied
- 2	Deploy Storage Account without ProjectName tag	❌ Denied
- 3	Create a Public IP	❌ Denied
- 4	Deploy VM in Canada Central with tag ProjectName=TestApp	✅ Allowed
  
Note: For each attempt, take screenshots of the result.
Denied actions will show a policy violation error.
The allowed VM should deploy successfully.

### ✅ Conclusion

By the end of this lab, you have:
- Created three custom Azure Policies to enforce governance.
- Grouped them into a Policy Initiative for easy management.
- Assigned the initiative at the resource group scope.
- Validated compliance through real-world test cases.
  
This ensures MapleTech Solutions maintains secure, compliant, and well-governed cloud deployments
