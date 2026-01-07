# Lab: Create and Delete Azure Resource Groups

## Objective

Demonstrate end‑to‑end lifecycle management of Azure Resource Groups (create, list, tag, and delete) using the Azure portal and Azure CLI, with focus on governance, cost control, and safe cleanup.  

---

## Concept Overview

- A **resource group (RG)** is a logical container that holds related Azure resources and provides a single scope for deployment, RBAC, policy, and lifecycle operations.  
- RGs are typically aligned to an **application, environment, or workload** (for example, `webapp-dev-rg`, `webapp-uat-rg`) rather than to individual services.   
- Tags on RGs (such as `Environment`, `Owner`, `CostCenter`) are critical for cost allocation and reporting in Cost Management.   

---  

## What I Performed in This Lab  

- Created resource groups in specific regions via portal and CLI:  
  - Portal: *Create resource group* then set name, region, and tags.   
  - CLI: 
```bash
az group create --name app-dev-rg --location eastus --tags Environment=Dev Owner=Ahmad
```

<img src="01-fundamentals/lab-01-resource-groups/rg-creation-CLI.png" alt="Resource group created using Cloud shell" width="80%">
- Listed and verified existing RGs and tags using:  
  - `az group list --query "[].{Name:name, Location:location, Tags:tags}" -o table`. 
- Deleted temporary / lab RGs to clean up costs:  
  - Portal: **Delete resource group** → type RG name to confirm. 
  - CLI: `az group delete --name <resource-group-name> --yes`.

---

## Key Learnings

- RG deletion is **asynchronous**: Azure Resource Manager determines dependency order, deletes nested resources first, then parent resources, so the RG can stay in *Deleting* state for some time.  
- A single RG‑level delete permission (`Microsoft.Resources/subscriptions/resourceGroups/delete`) is enough to remove the group and all contained resources, even if the role lacks delete permissions on each resource type individually.  
- Applying **CanNotDelete** locks at RG level protects from accidental RG deletion but can also block automated cleanup (for example, backup restore points and deployment history) if not planned carefully.  

---

## Troubleshooting Deletion Issues

- If RG deletion fails or is stuck, first check and remove **resource locks**:  
  - Portal: **Resource group then click  Locks**.
  - CLI:  
    - List locks: `az lock list --resource-group <rg-name>`.  
    - Delete lock: `az lock delete --ids <lock-id>`. 
- Verify RBAC: ensure the user or service principal has RG delete permission; missing rights appear as authorization errors in activity log or CLI output.   
- If specific resources block deletion (for example, failed deployments, dependent resources, or provider issues), delete or fix those resources individually, then retry RG deletion; persistent issues may require Azure Support.  

