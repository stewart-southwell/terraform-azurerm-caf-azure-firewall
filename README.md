[![Build status](https://dev.azure.com/azure-terraform/Blueprints/_apis/build/status/modules/azure_firewall)](https://dev.azure.com/azure-terraform/Blueprints/_build/latest?definitionId=11)
# Deploys Azure Firewall
Creates an Azure Firewall in a given region


Reference the module to a specific version (recommended):
```hcl
module "az_firewall" {
    source  = "aztfmod/caf-azure-firewall/azurerm"
    version = "0.x.y"

    az_fw_name                        = var.az_fw_name
    az_fw_rg                          = var.virtual_network_rg
    subnet_id                         = var.subnetid
    public_ip_id                      = var.pip.id
    location                          = var.location["region1"]
    tags                              = var.tags
    diagnostics_map                   = var.diagnostics_map
    log_analytics_workspace_id           = var.log_analytics_workspace
}
```

# Parameters
## name
(Required) Name of the Azure Firewall to be created
```hcl
variable "name" {
  description = "(Required) Name of the Azure Firewall to be created"  
}

```
Example
```hcl
name = "arnaud-firewall"
```

## rg
(Required) Resource Group of the Azure Firewall to be created.
```hcl
variable "rg" {
  description = "(Required) Resource Group of the Azure Firewall to be created"  
}

```
Example
```hcl
rg = "operations-rg"
```

## subnet_id
(Required) ID for the subnet where to deploy the Azure Firewall
```hcl
variable "subnet_id" {
  description = "(Required) ID for the subnet where to deploy the Azure Firewall " 
}

```
Example
```hcl
subnet_id = "/subscriptions/00000000-0000-0000-aa36-000000000000/resourceGroups/ftau-HUB-CORE-NET/providers/Microsoft.Network/virtualNetworks/ftau-Shared-Services/subnets/AzureFirewallSubnet"
```


## public_ip_id
(Required) ID for the subnet where to deploy the Azure Firewall
```hcl
variable "public_ip_id" {
  description = "(Required) Public IP address identifier. IP address must be of type static and standard."
}
```
Example
```hcl
public_ip_id = "/subscriptions/00000000-0000-0000-aa36-000000000000/resourceGroups/ftau-HUB-EDGE-NETS/providers/Microsoft.Network/publicIPAddresses/az_fw_pip"
```

## location
(Required) Define the region where the Azure Firewall will be created.
```hcl

variable "location" {
  description = "(Required) Define the region where the Azure Firewall vault will be created"
  type        = string
}
```
Example
```hcl
    location    = "southeastasia"
```

## tags
(Required) Map of tags for the deployment
```hcl
variable "tags" {
  description = "(Required) map of tags for the deployment"
}
```
Example
```hcl
tags = {
    environment     = "DEV"
    owner           = "Arnaud"
    deploymentType  = "Terraform"
  }
```

## diagnostics_map
(Required) Map with the diagnostics repository information"
```hcl
variable "diagnostics_map" {
 description = "(Required) Map with the diagnostics repository information"
}
```
Example
```hcl
  diagnostics_map = {
      diags_sa      = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/arnaud-hub-operations/providers/Microsoft.Storage/storageAccounts/opslogskumowxv"
      eh_id         = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/arnaud-hub-operations/providers/Microsoft.EventHub/namespaces/opslogskumowxv"
      eh_name       = "opslogskumowxv"
  }
```
## log_analytics_workspace_id
(Required) Log Analytics Workspace details
```hcl
variable "log_analytics_workspace_id" {
  description = "(Required) Log Analytics ID for the AzFW diagnostics"
}
```
Example
```hcl
  log_analytics_workspace_id =  "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/ftau-hub-operations/providers/microsoft.operationalinsights/workspaces/ftaulevel1"
```

## diagnostics_settings
(Required) Map with the settings for diagnostics of Azure Firewall
```hcl
variable "diagnostics_settings" {
 description = "(Required) Map with the diagnostics repository information"
}
```
Example

```hcl
diagnostics_settings = {
    log = [
                #["Category name",  "Diagnostics Enabled(true/false)", "Retention Enabled(true/false)", Retention_period] 
                ["AzureFirewallApplicationRule", true, true, 30],
                ["AzureFirewallNetworkRule", true, true, 30],
        ]
    metric = [
               ["AllMetrics", true, true, 30],
    ]
}
```

## convention
(Required) Naming convention to be used.
```hcl
variable "convention" {
  description = "(Required) Naming convention used"
}
```
Example
```hcl
convention = "cafclassic"
```

# Output

| Name | Type | Description | 
| -- | -- | -- | 
| object | object | Returns the full object of the created Azure Firewall. |
| name | string | Returns the name of the created Azure Firewall. |
| id | string | Returns the ID of the created Azure Firewall. | 
| az_firewall_config | map | Returns the Azure Firewall object with following attributes: <br> This is an old output kept for compatibility reason with v0.1 which might nolonger be supported in future versions. <br> It outputs: <br> - az_fw_name <br> -az_fw_id <br> -az_ipconfig <br> -az_object. |