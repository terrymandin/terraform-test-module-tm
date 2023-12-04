<!-- BEGIN_TF_DOCS -->
# terraform-azurerm-avm-template

This is a template repo for Terraform Azure Verified Modules.

Things to do:

1. Set up a GitHub repo environment called `test`.
1. Configure environment protection rule to ensure that approval is required before deploying to this environment.
1. Create a user-assigned managed identity in your test subscription.
1. Create a role assignment for the managed identity on your test subscription, use the minimum required role.
1. Configure federated identity credentials on the user assigned managed identity. Use the GitHub environment.
1. Create the following environment secrets on the `test` environment:
   1. AZURE\_CLIENT\_ID
   1. AZURE\_TENANT\_ID
   1. AZURE\_SUBSCRIPTION\_ID

Major version Zero (0.y.z) is for initial development. Anything MAY change at any time. A module SHOULD NOT be considered stable till at least it is major version one (1.0.0) or greater. Changes will always be via new versions being published and no changes will be made to existing published versions. For more details please go to https://semver.org/

<!-- markdownlint-disable MD033 -->
## Requirements

No requirements.

## Providers

The following providers are used by this module:

- <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm)

## Resources

The following resources are used by this module:

- [azurerm_orchestrated_virtual_machine_scale_set.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/orchestrated_virtual_machine_scale_set) (resource)

<!-- markdownlint-disable MD013 -->
## Required Inputs

The following input variables are required:

### <a name="input_location"></a> [location](#input\_location)

Description: (Required) The Azure location where the Orchestrated Virtual Machine Scale Set should exist. Changing this forces a new resource to be created.
-> Test Note

Type: `string`

### <a name="input_name"></a> [name](#input\_name)

Description: (Required) The name of the Orchestrated Virtual Machine Scale Set. Changing this forces a new resource to be created.

Type: `string`

### <a name="input_platform_fault_domain_count"></a> [platform\_fault\_domain\_count](#input\_platform\_fault\_domain\_count)

Description: (Required) Specifies the number of fault domains that are used by this Orchestrated Virtual Machine Scale Set. Changing this forces a new resource to be created.
<br>**NOTE:** The number of Fault Domains varies depending on which Azure Region you're using - a list can be found [here](https://github.com/MicrosoftDocs/azure-docs/blob/master/includes/managed-disks-common-fault-domain-region-list.md).

Type: `number`

### <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name)

Description: (Required) The name of the Resource Group in which the Orchestrated Virtual Machine Scale Set should exist. Changing this forces a new resource to be created.

Type: `string`

## Optional Inputs

The following input variables are optional (have default values):

### <a name="input_additional_capabilities"></a> [additional\_capabilities](#input\_additional\_capabilities)

Description: - `ultra_ssd_enabled` - (Optional) Should the capacity to enable Data Disks of the `UltraSSD_LRS` storage account type be supported on this Orchestrated Virtual Machine Scale Set? Defaults to `false`. Changing this forces a new resource to be created.

Type:

```hcl
object({
    ultra_ssd_enabled = optional(bool)
  })
```

Default: `null`

### <a name="input_automatic_instance_repair"></a> [automatic\_instance\_repair](#input\_automatic\_instance\_repair)

Description: Description: Enable the ability for the Virtual Machine Scale Set to repair the associated instances automatically.
<br><br>**NOTE:** To enable the automatic\_instance\_repair, the Orchestrated Virtual Machine Scale Set must have a valid health\_probe\_id or an [Application Health Extension](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension).
 - `enabled` - (Required) Should the automatic instance repair be enabled on this Orchestrated Virtual Machine Scale Set? Possible values are `true` and `false`.
 - `grace_period` - (Optional) Amount of time for which automatic repairs will be delayed. The grace period starts right after the VM is found unhealthy. Possible values are between `30` and `90` minutes. The time duration should be specified in `ISO 8601` format (e.g. `PT30M` to `PT90M`). Defaults to `PT30M`.

Type:

```hcl
object({
    enabled      = bool
    grace_period = optional(string)
  })
```

Default: `null`

### <a name="input_boot_diagnostics"></a> [boot\_diagnostics](#input\_boot\_diagnostics)

Description: - `storage_account_uri` - (Optional) The Primary/Secondary Endpoint for the Azure Storage Account which should be used to store Boot Diagnostics, including Console Output and Screenshots from the Hypervisor. By including a `boot_diagnostics` block without passing the `storage_account_uri` field will cause the API to utilize a Managed Storage Account to store the Boot Diagnostics output.

Type:

```hcl
object({
    storage_account_uri = optional(string)
  })
```

Default: `null`

### <a name="input_capacity_reservation_group_id"></a> [capacity\_reservation\_group\_id](#input\_capacity\_reservation\_group\_id)

Description: (Optional) Specifies the ID of the Capacity Reservation Group which the Virtual Machine Scale Set should be allocated to. Changing this forces a new resource to be created.

Type: `string`

Default: `null`

### <a name="input_data_disk"></a> [data\_disk](#input\_data\_disk)

Description: - `caching` - (Required) The type of Caching which should be used for this Data Disk. Possible values are None, ReadOnly and ReadWrite.
- `create_option` - (Optional) The create option which should be used for this Data Disk. Possible values are Empty and FromImage. Defaults to `Empty`. (FromImage should only be used if the source image includes data disks).
- `disk_encryption_set_id` - (Optional) The ID of the Disk Encryption Set which should be used to encrypt the Data Disk. Changing this forces a new resource to be created.
- `disk_size_gb` - (Required) The size of the Data Disk which should be created.
- `lun` - (Required) The Logical Unit Number of the Data Disk, which must be unique within the Virtual Machine.
- `storage_account_type` - (Required) The Type of Storage Account which should back this Data Disk. Possible values include `Standard_LRS`, `StandardSSD_LRS`, `StandardSSD_ZRS`, `Premium_LRS`, `PremiumV2_LRS`, `Premium_ZRS` and `UltraSSD_LRS`.
- `ultra_ssd_disk_iops_read_write` - (Optional) Specifies the Read-Write IOPS for this Data Disk. Only settable when `storage_account_type` is `PremiumV2_LRS` or `UltraSSD_LRS`.
- `ultra_ssd_disk_mbps_read_write` - (Optional) Specifies the bandwidth in MB per second for this Data Disk. Only settable when `storage_account_type` is `PremiumV2_LRS` or `UltraSSD_LRS`.
- `write_accelerator_enabled` - (Optional) Specifies if Write Accelerator is enabled on the Data Disk. Defaults to `false`.

Type:

```hcl
list(object({
    caching                        = string
    create_option                  = optional(string)
    disk_encryption_set_id         = optional(string)
    disk_size_gb                   = number
    lun                            = number
    storage_account_type           = string
    ultra_ssd_disk_iops_read_write = optional(number)
    ultra_ssd_disk_mbps_read_write = optional(number)
    write_accelerator_enabled      = optional(bool)
  }))
```

Default: `null`

### <a name="input_encryption_at_host_enabled"></a> [encryption\_at\_host\_enabled](#input\_encryption\_at\_host\_enabled)

Description: (Optional) Should disks attached to this Virtual Machine Scale Set be encrypted by enabling Encryption at Host?

Type: `bool`

Default: `null`

### <a name="input_eviction_policy"></a> [eviction\_policy](#input\_eviction\_policy)

Description: (Optional) The Policy which should be used Virtual Machines are Evicted from the Scale Set. Possible values are `Deallocate` and `Delete`. Changing this forces a new resource to be created.

Type: `string`

Default: `null`

### <a name="input_extension"></a> [extension](#input\_extension)

Description: - `auto_upgrade_minor_version_enabled` - (Optional) Should the latest version of the Extension be used at Deployment Time, if one is available? This won't auto-update the extension on existing installation. Defaults to `true`.
- `extensions_to_provision_after_vm_creation` - (Optional) An ordered list of Extension names which Orchestrated Virtual Machine Scale Set should provision after VM creation.
- `failure_suppression_enabled` - (Optional) Should failures from the extension be suppressed? Possible values are `true` or `false`.
- `force_extension_execution_on_change` - (Optional) A value which, when different to the previous value can be used to force-run the Extension even if the Extension Configuration hasn't changed.
- `name` - (Required) The name for the Virtual Machine Scale Set Extension.
- `protected_settings` - (Optional) A JSON String which specifies Sensitive Settings (such as Passwords) for the Extension.
- `publisher` - (Required) Specifies the Publisher of the Extension.
- `settings` - (Optional) A JSON String which specifies Settings for the Extension.
- `type` - (Required) Specifies the Type of the Extension.
- `type_handler_version` - (Required) Specifies the version of the extension to use, available versions can be found using the Azure CLI.

---
`protected_settings_from_key_vault` block supports the following:
- `secret_url` - (Required) The URL to the Key Vault Secret which stores the protected settings.
- `source_vault_id` - (Required) The ID of the source Key Vault.

Type:

```hcl
set(object({
    auto_upgrade_minor_version_enabled        = optional(bool)
    extensions_to_provision_after_vm_creation = optional(list(string))
    failure_suppression_enabled               = optional(bool)
    force_extension_execution_on_change       = optional(string)
    name                                      = string
    protected_settings                        = optional(string)
    publisher                                 = string
    settings                                  = optional(string)
    type                                      = string
    type_handler_version                      = string
    protected_settings_from_key_vault = optional(object({
      secret_url      = string
      source_vault_id = string
    }))
  }))
```

Default: `null`

### <a name="input_extension_operations_enabled"></a> [extension\_operations\_enabled](#input\_extension\_operations\_enabled)

Description: (Optional) Should extension operations be allowed on the Virtual Machine Scale Set? Possible values are `true` or `false`. Defaults to `true`. Changing this forces a new Orchestrated Virtual Machine Scale Set to be created.

Type: `bool`

Default: `null`

### <a name="input_extensions_time_budget"></a> [extensions\_time\_budget](#input\_extensions\_time\_budget)

Description: (Optional) Specifies the time alloted for all extensions to start. The time duration should be between 15 minutes and 120 minutes (inclusive) and should be specified in ISO 8601 format. Defaults to `PT1H30M`.

Type: `string`

Default: `null`

### <a name="input_identity"></a> [identity](#input\_identity)

Description: - `identity_ids` - (Required) Specifies a list of User Managed Identity IDs to be assigned to this Orchestrated Windows Virtual Machine Scale Set.
- `type` - (Required) The type of Managed Identity that should be configured on this Orchestrated Windows Virtual Machine Scale Set. Only possible value is `UserAssigned`.

Type:

```hcl
object({
    identity_ids = set(string)
    type         = string
  })
```

Default: `null`

### <a name="input_instances"></a> [instances](#input\_instances)

Description: (Optional) The number of Virtual Machines in the Orcestrated Virtual Machine Scale Set.

Type: `number`

Default: `null`

### <a name="input_license_type"></a> [license\_type](#input\_license\_type)

Description: (Optional) Specifies the type of on-premise license (also known as Azure Hybrid Use Benefit) which should be used for this Orchestrated Virtual Machine Scale Set. Possible values are `None`, `Windows_Client` and `Windows_Server`.

Type: `string`

Default: `null`

### <a name="input_max_bid_price"></a> [max\_bid\_price](#input\_max\_bid\_price)

Description: (Optional) The maximum price you're willing to pay for each Orchestrated Virtual Machine in this Scale Set, in US Dollars; which must be greater than the current spot price. If this bid price falls below the current spot price the Virtual Machines in the Scale Set will be evicted using the eviction\_policy. Defaults to `-1`, which means that each Virtual Machine in the Orchestrated Scale Set should not be evicted for price reasons.

Type: `number`

Default: `null`

### <a name="input_network_interface"></a> [network\_interface](#input\_network\_interface)

Description: - `dns_servers` - (Optional) A list of IP Addresses of DNS Servers which should be assigned to the Network Interface.
- `enable_accelerated_networking` - (Optional) Does this Network Interface support Accelerated Networking? Possible values are `true` and `false`. Defaults to `false`.
- `enable_ip_forwarding` - (Optional) Does this Network Interface support IP Forwarding? Possible values are `true` and `false`. Defaults to `false`.
- `name` - (Required) The Name which should be used for this Network Interface. Changing this forces a new resource to be created.
- `network_security_group_id` - (Optional) The ID of a Network Security Group which should be assigned to this Network Interface.
- `primary` - (Optional) Is this the Primary IP Configuration? Possible values are `true` and `false`. Defaults to `false`.

---
`ip_configuration` block supports the following:
- `application_gateway_backend_address_pool_ids` - (Optional) A list of Backend Address Pools IDs from a Application Gateway which this Orchestrated Virtual Machine Scale Set should be connected to.
- `application_security_group_ids` - (Optional) A list of Application Security Group IDs which this Orchestrated Virtual Machine Scale Set should be connected to.
- `load_balancer_backend_address_pool_ids` - (Optional) A list of Backend Address Pools IDs from a Load Balancer which this Orchestrated Virtual Machine Scale Set should be connected to.
- `name` - (Required) The Name which should be used for this IP Configuration.
- `primary` - (Optional) Is this the Primary IP Configuration for this Network Interface? Possible values are `true` and `false`. Defaults to `false`.
- `subnet_id` - (Optional) The ID of the Subnet which this IP Configuration should be connected to.
- `version` - (Optional) The Internet Protocol Version which should be used for this IP Configuration. Possible values are `IPv4` and `IPv6`. Defaults to `IPv4`.

---
`public_ip_address` block supports the following:
- `domain_name_label` - (Optional) The Prefix which should be used for the Domain Name Label for each Virtual Machine Instance. Azure concatenates the Domain Name Label and Virtual Machine Index to create a unique Domain Name Label for each Virtual Machine. Valid values must be between `1` and `26` characters long, start with a lower case letter, end with a lower case letter or number and contains only `a-z`, `0-9` and `hyphens`.
- `idle_timeout_in_minutes` - (Optional) The Idle Timeout in Minutes for the Public IP Address. Possible values are in the range `4` to `32`.
- `name` - (Required) The Name of the Public IP Address Configuration.
- `public_ip_prefix_id` - (Optional) The ID of the Public IP Address Prefix from where Public IP Addresses should be allocated. Changing this forces a new resource to be created.
- `sku_name` - (Optional) Specifies what Public IP Address SKU the Public IP Address should be provisioned as. Possible vaules include `Basic_Regional`, `Basic_Global`, `Standard_Regional` or `Standard_Global`. For more information about Public IP Address SKU's and their capabilities, please see the [product documentation](https://docs.microsoft.com/azure/virtual-network/ip-services/public-ip-addresses#sku). Changing this forces a new resource to be created.
- `version` - (Optional) The Internet Protocol Version which should be used for this public IP address. Possible values are `IPv4` and `IPv6`. Defaults to `IPv4`. Changing this forces a new resource to be created.

---
`ip_tag` block supports the following:
- `tag` - (Required) The IP Tag associated with the Public IP, such as `SQL` or `Storage`. Changing this forces a new resource to be created.
- `type` - (Required) The Type of IP Tag, such as `FirstPartyUsage`. Changing this forces a new resource to be created.

Type:

```hcl
list(object({
    dns_servers                   = optional(list(string))
    enable_accelerated_networking = optional(bool)
    enable_ip_forwarding          = optional(bool)
    name                          = string
    network_security_group_id     = optional(string)
    primary                       = optional(bool)
    ip_configuration = list(object({
      application_gateway_backend_address_pool_ids = optional(set(string))
      application_security_group_ids               = optional(set(string))
      load_balancer_backend_address_pool_ids       = optional(set(string))
      name                                         = string
      primary                                      = optional(bool)
      subnet_id                                    = optional(string)
      version                                      = optional(string)
      public_ip_address = optional(list(object({
        domain_name_label       = optional(string)
        idle_timeout_in_minutes = optional(number)
        name                    = string
        public_ip_prefix_id     = optional(string)
        sku_name                = optional(string)
        version                 = optional(string)
        ip_tag = optional(list(object({
          tag  = string
          type = string
        })))
      })))
    }))
  }))
```

Default: `null`

### <a name="input_os_disk"></a> [os\_disk](#input\_os\_disk)

Description: - `caching` - (Required) The Type of Caching which should be used for the Internal OS Disk. Possible values are `None`, `ReadOnly` and `ReadWrite`.
- `disk_encryption_set_id` - (Optional) The ID of the Disk Encryption Set which should be used to encrypt this OS Disk. Changing this forces a new resource to be created.
- `disk_size_gb` - (Optional) The Size of the Internal OS Disk in GB, if you wish to vary from the size used in the image this Virtual Machine Scale Set is sourced from.
- `storage_account_type` - (Required) The Type of Storage Account which should back this the Internal OS Disk. Possible values include `Standard_LRS`, `StandardSSD_LRS`, `StandardSSD_ZRS`, `Premium_LRS` and `Premium_ZRS`. Changing this forces a new resource to be created.
- `write_accelerator_enabled` - (Optional) Specifies if Write Accelerator is enabled on the OS Disk. Defaults to `false`.

---
`diff_disk_settings` block supports the following:
- `option` - (Required) Specifies the Ephemeral Disk Settings for the OS Disk. At this time the only possible value is `Local`. Changing this forces a new resource to be created.
- `placement` - (Optional) Specifies where to store the Ephemeral Disk. Possible values are `CacheDisk` and `ResourceDisk`. Defaults to `CacheDisk`. Changing this forces a new resource to be created.

Type:

```hcl
object({
    caching                   = string
    disk_encryption_set_id    = optional(string)
    disk_size_gb              = optional(number)
    storage_account_type      = string
    write_accelerator_enabled = optional(bool)
    diff_disk_settings = optional(object({
      option    = string
      placement = optional(string)
    }))
  })
```

Default: `null`

### <a name="input_os_profile"></a> [os\_profile](#input\_os\_profile)

Description: - `custom_data` - (Optional) The Base64-Encoded Custom Data which should be used for this Orchestrated Virtual Machine Scale Set.

---
`linux_configuration` block supports the following:
- `admin_password` - (Optional) The Password which should be used for the local-administrator on this Virtual Machine. Changing this forces a new resource to be created.
- `admin_username` - (Required) The username of the local administrator on each Orchestrated Virtual Machine Scale Set instance. Changing this forces a new resource to be created.
- `computer_name_prefix` - (Optional) The prefix which should be used for the name of the Virtual Machines in this Scale Set. If unspecified this defaults to the value for the name field. If the value of the name field is not a valid `computer_name_prefix`, then you must specify `computer_name_prefix`. Changing this forces a new resource to be created.
- `disable_password_authentication` - (Optional) When an `admin_password` is specified `disable_password_authentication` must be set to `false`. Defaults to `true`.
- `patch_assessment_mode` - (Optional) Specifies the mode of VM Guest Patching for the virtual machines that are associated to the Orchestrated Virtual Machine Scale Set. Possible values are `AutomaticByPlatform` or `ImageDefault`. Defaults to `ImageDefault`.
- `patch_mode` - (Optional) Specifies the mode of in-guest patching of this Windows Virtual Machine. Possible values are `ImageDefault` or `AutomaticByPlatform`. Defaults to `ImageDefault`. For more information on patch modes please see the [product documentation](https://docs.microsoft.com/azure/virtual-machines/automatic-vm-guest-patching#patch-orchestration-modes).
- `provision_vm_agent` - (Optional) Should the Azure VM Agent be provisioned on each Virtual Machine in the Scale Set? Defaults to `true`. Changing this value forces a new resource to be created.

---
`admin_ssh_key` block supports the following:
- `public_key` - (Required) The Public Key which should be used for authentication, which needs to be at least 2048-bit and in ssh-rsa format.
- `username` - (Required) The Username for which this Public SSH Key should be configured.

---
`secret` block supports the following:
- `key_vault_id` - (Required) The ID of the Key Vault from which all Secrets should be sourced.

---
`certificate` block supports the following:
- `url` - (Required) The Secret URL of a Key Vault Certificate.

---
`windows_configuration` block supports the following:
- `admin_password` - (Required) The Password which should be used for the local-administrator on this Virtual Machine. Changing this forces a new resource to be created.
- `admin_username` - (Required) The username of the local administrator on each Orchestrated Virtual Machine Scale Set instance. Changing this forces a new resource to be created.
- `computer_name_prefix` - (Optional) The prefix which should be used for the name of the Virtual Machines in this Scale Set. If unspecified this defaults to the value for the `name` field. If the value of the `name` field is not a valid `computer_name_prefix`, then you must specify `computer_name_prefix`. Changing this forces a new resource to be created.
- `enable_automatic_updates` - (Optional) Are automatic updates enabled for this Virtual Machine? Defaults to `true`.
- `hotpatching_enabled` - (Optional) Should the VM be patched without requiring a reboot? Possible values are `true` or `false`. Defaults to `false`. For more information about hot patching please see the [product documentation](https://docs.microsoft.com/azure/automanage/automanage-hotpatch).
- `patch_assessment_mode` - (Optional) Specifies the mode of VM Guest Patching for the virtual machines that are associated to the Orchestrated Virtual Machine Scale Set. Possible values are `AutomaticByPlatform` or `ImageDefault`. Defaults to `ImageDefault`.
- `patch_mode` - (Optional) Specifies the mode of in-guest patching of this Windows Virtual Machine. Possible values are `Manual`, `AutomaticByOS` and `AutomaticByPlatform`. Defaults to `AutomaticByOS`. For more information on patch modes please see the [product documentation](https://docs.microsoft.com/azure/virtual-machines/automatic-vm-guest-patching#patch-orchestration-modes).
- `provision_vm_agent` - (Optional) Should the Azure VM Agent be provisioned on each Virtual Machine in the Scale Set? Defaults to `true`. Changing this value forces a new resource to be created.
- `timezone` - (Optional) Specifies the time zone of the virtual machine, the possible values are defined [here](https://jackstromberg.com/2017/01/list-of-time-zones-consumed-by-azure/).

---
`secret` block supports the following:
- `key_vault_id` - (Required) The ID of the Key Vault from which all Secrets should be sourced.

---
`certificate` block supports the following:
- `store` - (Required) The certificate store on the Virtual Machine where the certificate should be added.
- `url` - (Required) The Secret URL of a Key Vault Certificate.

---
`winrm_listener` block supports the following:
- `certificate_url` - (Optional) The Secret URL of a Key Vault Certificate, which must be specified when protocol is set to `Https`. Changing this forces a new resource to be created.
- `protocol` - (Required) Specifies the protocol of listener. Possible values are `Http` or `Https`. Changing this forces a new resource to be created.

Type:

```hcl
object({
    custom_data = optional(string)
    linux_configuration = optional(object({
      admin_password                  = optional(string)
      admin_username                  = string
      computer_name_prefix            = optional(string)
      disable_password_authentication = optional(bool)
      patch_assessment_mode           = optional(string)
      patch_mode                      = optional(string)
      provision_vm_agent              = optional(bool)
      admin_ssh_key = optional(set(object({
        public_key = string
        username   = string
      })))
      secret = optional(list(object({
        key_vault_id = string
        certificate = set(object({
          url = string
        }))
      })))
    }))
    windows_configuration = optional(object({
      admin_password           = string
      admin_username           = string
      computer_name_prefix     = optional(string)
      enable_automatic_updates = optional(bool)
      hotpatching_enabled      = optional(bool)
      patch_assessment_mode    = optional(string)
      patch_mode               = optional(string)
      provision_vm_agent       = optional(bool)
      timezone                 = optional(string)
      secret = optional(list(object({
        key_vault_id = string
        certificate = set(object({
          store = string
          url   = string
        }))
      })))
      winrm_listener = optional(set(object({
        certificate_url = optional(string)
        protocol        = string
      })))
    }))
  })
```

Default: `null`

### <a name="input_plan"></a> [plan](#input\_plan)

Description: - `name` - (Required) Specifies the name of the image from the marketplace. Changing this forces a new resource to be created.
- `product` - (Required) Specifies the product of the image from the marketplace. Changing this forces a new resource to be created.
- `publisher` - (Required) Specifies the publisher of the image. Changing this forces a new resource to be created.

Type:

```hcl
object({
    name      = string
    product   = string
    publisher = string
  })
```

Default: `null`

### <a name="input_priority"></a> [priority](#input\_priority)

Description: (Optional) The Priority of this Orchestrated Virtual Machine Scale Set. Possible values are `Regular` and `Spot`. Defaults to `Regular`. Changing this value forces a new resource.

Type: `string`

Default: `null`

### <a name="input_priority_mix"></a> [priority\_mix](#input\_priority\_mix)

Description: - `base_regular_count` - (Optional) Specifies the base number of VMs of `Regular` priority that will be created before any VMs of priority `Spot` are created. Possible values are integers between `0` and `1000`. Defaults to `0`.
- `regular_percentage_above_base` - (Optional) Specifies the desired percentage of VM instances that are of `Regular` priority after the base count has been reached. Possible values are integers between `0` and `100`. Defaults to `0`.

Type:

```hcl
object({
    base_regular_count            = optional(number)
    regular_percentage_above_base = optional(number)
  })
```

Default: `null`

### <a name="input_proximity_placement_group_id"></a> [proximity\_placement\_group\_id](#input\_proximity\_placement\_group\_id)

Description: (Optional) The ID of the Proximity Placement Group which the Orchestrated Virtual Machine should be assigned to. Changing this forces a new resource to be created.

Type: `string`

Default: `null`

### <a name="input_single_placement_group"></a> [single\_placement\_group](#input\_single\_placement\_group)

Description: (Optional) Should this Virtual Machine Scale Set be limited to a Single Placement Group, which means the number of instances will be capped at 100 Virtual Machines. Possible values are `true` or `false`.

Type: `bool`

Default: `null`

### <a name="input_sku_name"></a> [sku\_name](#input\_sku\_name)

Description: (Optional) The `name` of the SKU to be used by this Orcestrated Virtual Machine Scale Set. Valid values include: any of the [General purpose](https://docs.microsoft.com/azure/virtual-machines/sizes-general), [Compute optimized](https://docs.microsoft.com/azure/virtual-machines/sizes-compute), [Memory optimized](https://docs.microsoft.com/azure/virtual-machines/sizes-memory), [Storage optimized](https://docs.microsoft.com/azure/virtual-machines/sizes-storage), [GPU optimized](https://docs.microsoft.com/azure/virtual-machines/sizes-gpu), [FPGA optimized](https://docs.microsoft.com/azure/virtual-machines/sizes-field-programmable-gate-arrays), [High performance](https://docs.microsoft.com/azure/virtual-machines/sizes-hpc), or [Previous generation](https://docs.microsoft.com/azure/virtual-machines/sizes-previous-gen) virtual machine SKUs.

Type: `string`

Default: `null`

### <a name="input_source_image_id"></a> [source\_image\_id](#input\_source\_image\_id)

Description: (Optional) The ID of an Image which each Virtual Machine in this Scale Set should be based on. Possible Image ID types include `Image ID`s, `Shared Image ID`s, `Shared Image Version ID`s, `Community Gallery Image ID`s, `Community Gallery Image Version ID`s, `Shared Gallery Image ID`s and `Shared Gallery Image Version ID`s.

Type: `string`

Default: `null`

### <a name="input_source_image_reference"></a> [source\_image\_reference](#input\_source\_image\_reference)

Description: - `offer` - (Required) Specifies the offer of the image used to create the virtual machines. Changing this forces a new resource to be created.
- `publisher` - (Required) Specifies the publisher of the image used to create the virtual machines. Changing this forces a new resource to be created.
- `sku` - (Required) Specifies the SKU of the image used to create the virtual machines.
- `version` - (Required) Specifies the version of the image used to create the virtual machines.

Type:

```hcl
object({
    offer     = string
    publisher = string
    sku       = string
    version   = string
  })
```

Default: `null`

### <a name="input_tags"></a> [tags](#input\_tags)

Description: (Optional) A mapping of tags which should be assigned to this Orchestrated Virtual Machine Scale Set.

Type: `map(string)`

Default: `null`

### <a name="input_termination_notification"></a> [termination\_notification](#input\_termination\_notification)

Description: - `enabled` - (Required) Should the termination notification be enabled on this Virtual Machine Scale Set? Possible values `true` or `false`
- `timeout` - (Optional) Length of time (in minutes, between `5` and `15`) a notification to be sent to the VM on the instance metadata server till the VM gets deleted. The time duration should be specified in `ISO 8601` format. Defaults to `PT5M`.

Type:

```hcl
object({
    enabled = bool
    timeout = optional(string)
  })
```

Default: `null`

### <a name="input_timeouts"></a> [timeouts](#input\_timeouts)

Description: - `create` - (Defaults to 60 minutes) Used when creating the Orchestrated Virtual Machine Scale Set.
- `delete` - (Defaults to 60 minutes) Used when deleting the Orchestrated Virtual Machine Scale Set.
- `read` - (Defaults to 5 minutes) Used when retrieving the Orchestrated Virtual Machine Scale Set.
- `update` - (Defaults to 60 minutes) Used when updating the Orchestrated Virtual Machine Scale Set.

Type:

```hcl
object({
    create = optional(string)
    delete = optional(string)
    read   = optional(string)
    update = optional(string)
  })
```

Default: `null`

### <a name="input_user_data_base64"></a> [user\_data\_base64](#input\_user\_data\_base64)

Description: (Optional) The Base64-Encoded User Data which should be used for this Virtual Machine Scale Set.

Type: `string`

Default: `null`

### <a name="input_zone_balance"></a> [zone\_balance](#input\_zone\_balance)

Description: (Optional) Should the Virtual Machines in this Scale Set be strictly evenly distributed across Availability Zones? Defaults to `false`. Changing this forces a new resource to be created.

Type: `bool`

Default: `null`

### <a name="input_zones"></a> [zones](#input\_zones)

Description: (Optional) Specifies a list of Availability Zones in which this Orchestrated Virtual Machine should be located. Changing this forces a new Orchestrated Virtual Machine to be created.

Type: `set(string)`

Default: `null`

## Outputs

No outputs.

## Modules

No modules.

<!-- markdownlint-disable-next-line MD041 -->
## Data Collection

The software may collect information about you and your use of the software and send it to Microsoft. Microsoft may use this information to provide services and improve our products and services. You may turn off the telemetry as described in the repository. There are also some features in the software that may enable you and Microsoft to collect data from users of your applications. If you use these features, you must comply with applicable law, including providing appropriate notices to users of your applications together with a copy of Microsoft’s privacy statement. Our privacy statement is located at <https://go.microsoft.com/fwlink/?LinkID=824704>. You can learn more about data collection and use in the help documentation and our privacy statement. Your use of the software operates as your consent to these practices.
<!-- END_TF_DOCS -->