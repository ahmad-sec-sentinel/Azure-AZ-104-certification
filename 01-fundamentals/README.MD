# AGENDA:
- UNDERSTANDING AZURE HIERARCHY( TENANT, ROOT MANAGEMENT GROUP, MANAGEMENT GROUP, SUBSCRIPTIONS)
- AZURE POLICY,AZURE ROLES
---  
## Overview  
Azure governance has three main things: a hierarchy that organizes everything, policies that enforce rules, and roles that control who can do what. Let me break each down simply.

---

## The Azure Hierarchy (4 Layers)

Think of Azure like a folder structure on your computer:

**1. Tenant**
- This is your entire Azure account/organization. One tenant per company/org.
- Everything below it (subscriptions, resources) belongs to this tenant.

**2. Management Groups** (optional layer)
- Used by larger companies to group multiple subscriptions together.
- Lets you apply policies and rules to many subscriptions at once instead of doing it one by one.
- Example: You have 10 production subscriptions. Instead of setting the same policy 10 times, you group them in a "Prod" management group and set it once.

**3. Subscriptions**
- Billing boundary. Each subscription is like a project or department with its own budget.
- Resources live inside subscriptions, not directly under a subscription.
- You can have multiple subscriptions, but each one is separate for billing purposes.

**4. Resource Groups**
- A container for your actual resources (VMs, databases, storage accounts, etc.).
- Every resource must belong to exactly one resource group.
- Helps you organize and manage related resources together.

**How policies flow down:** A policy set at the Tenant level applies to all subscriptions and resource groups below it. A policy at Subscription level applies to all resource groups in that subscription. This is called inheritance.

---

## Azure Policy (Enforcing Rules)

Azure Policy is how you enforce rules across your resources. Instead of checking manually if everyone followed the rules, Azure does it automatically.

### What can policies do?

- **Deny:** Block creation of resources that don't follow the rule. Example: "No VM without encryption."
- **Audit:** Allow creation but log which resources don't follow the rule. Example: "You can create it, but we'll report if it's missing encryption."
- **Append:** Automatically add something to resources. Example: "Automatically add tags to every resource."
- **DeployIfNotExists:** If a resource needs something (like a monitoring agent), automatically add it.

### Example

Your company wants all VMs to have encryption. Instead of telling each team "remember to enable encryption," you create a policy that:
- Either blocks unencrypted VMs from being created, or
- Allows creation but warns you to enable encryption.

You set this policy once at the Subscription level, and it applies to all resource groups under it.

---

## Azure Roles (Who Can Do What)

Roles control permissions. Each person gets a role that lets them do certain things.

### Common Built-In Roles

- **Owner:** Can do everything (create, delete, change permissions). Use this carefully.
- **Contributor:** Can create and modify resources but can't change who has access.
- **Reader:** Can only view things, not change anything.

### How Roles Work

You assign roles to people at different levels:
- Assign Owner at Subscription level → person owns that whole subscription.
- Assign Contributor at Resource Group level → person can manage resources only in that group.
- Assign Reader at Management Group level → person can view everything under that MG.

### Least Privilege (Important Concept)

Give people only the access they need, nothing more. If a developer only needs to manage VMs, don't give them database deletion rights.

---

## How They Work Together

Let's say you have a company with:
- A Production subscription (real customer data)
- A Development subscription (testing, learning)

**Here's what you'd do:**

1. **Hierarchy:** Put both subscriptions under management groups (Prod MG and Dev MG).

2. **Policy:** 
   - At Prod MG: Set a policy that denies deletion of resources (safety).
   - At Dev MG: Set a policy that allows full freedom (developers can experiment).

3. **Roles:**
   - Give developers "Contributor" on Dev subscription only (so they can build freely).
   - Give them "Reader" on Prod subscription (so they can see it but not break it).
   - Give a senior engineer "Owner" on Prod to manage critical changes.

Now your rules are enforced automatically. Developers can't delete production resources because:
1. The policy denies deletion in Prod.
2. They only have Reader access there anyway.

---

## Key Takeaways

- **Hierarchy:** Tenant → Management Group → Subscription → Resource Group → Resources
- **Policies:** Rules that enforce compliance automatically (Deny, Audit, Append, etc.)
- **Roles:** Who has access and what they can do (Owner, Contributor, Reader)
- **Inheritance:** Policies and rules flow down from top to bottom
- **Least Privilege:** Give minimal access needed

---

## Common Mistakes to Avoid

- Giving everyone Owner role (risky, breaks compliance).
- Setting policies at every level (creates confusion; set once, inherit).
- Not using resource groups (hard to manage and track costs).
- Forgetting that Resource Group can't be moved between subscriptions (choose carefully).

---

## What I Learned in This Lab

I practiced creating policies that enforce encryption, tested how roles control access at different levels, and saw how management groups make it easy to apply rules across many subscriptions without repetition. When a policy is set at a higher level and inherited, it's much easier to manage at scale than doing it manually for each subscription.


