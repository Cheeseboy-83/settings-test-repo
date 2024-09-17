# Overview
This module builds a resource group with the required tags imbedded.

## Requirements
No requirements

## Providers
| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [azurerm_resource_group.rg](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_name"></a> [name](#input\_name) | Resource Group Name | `string` | n/a | yes |
| <a name="input_location"></a> [location](#input\_location) | The Azure Region where the Resource Group should exist | `string` | n/a | yes |
| <a name="input_tags"></a> [tags](#input\_tags) | Tags | `list` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_id"></a> [id](#output\_id) | n/a |
| <a name="output_location"></a> [location](#output\_location) | n/a |
| <a name="output_name"></a> [name](#output\_name) | n/a |

## Versions

#### ***This module was previously tested with the below version for Terraform and AzureRM

| Name | Version |
|------|---------|
|Terraform | 1.9.5 |
|Azurerm | 3.109.0 |

### How to use

Simply copy the code below 

```
module "resource-group" {
  source  = "app.terraform.io/cheeseboy/resource-group/azurerm"
  version = "0.1.0"                                                     # replace with latest/desired version
  for_each = { for key, value in var.resource_groups : key => value }

  name     = each.value.name
  location = each.value.location
  tags     = merge(var.required_tags, each.value.tags)
}
```

You must also include the following variable declaration in your `variables.tf`:

```
variable "resource_groups" {
  type = map(object({
    name     = string
    location = string
    tags     = optional(map(string))
  }))
  description = "Resource groups in the deployment"
}
```

This will allow the module to consume the variable properly. You must also define the variable in your `resource_groups.tfvars` file:
```
resource_groups = {
  resource_group_0 = {
    name     = "rg_0_name"
    location = "rg_0_location"
    tags     = {
      key0 = "value0"
      key1 = "value1"
    }
  },
  resource_group_1 = {
    name     = "rg_1_name"
    location = "rg_1_location"
    tags     = {
      key0 = "value0"
      key1 = "value1"
    }
  }
}
```
