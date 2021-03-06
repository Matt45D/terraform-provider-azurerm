---
subcategory: "Container"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_container_service"
description: |-
  Manages an Azure Container Service instance.
---

# azurerm_container_service

Manages an Azure Container Service Instance

~> **NOTE:** All arguments including the client secret will be stored in the raw state as plain-text.
[Read more about sensitive data in state](/docs/state/sensitive-data.html).

~> **DEPRECATED:** [Azure Container Service (ACS) has been deprecated by Azure in favour of Azure (Managed) Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/updates/azure-container-service-will-retire-on-january-31-2020/). Support for ACS will be removed in the next major version of the AzureRM Provider (2.0) - and we **strongly recommend** you consider using [Azure Kubernetes Service (AKS)](kubernetes_cluster.html) for new deployments.

## Example Usage (DCOS)

```hcl
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West US"
}

resource "azurerm_container_service" "example" {
  name                   = "acctestcontservice1"
  location               = azurerm_resource_group.example.location
  resource_group_name    = azurerm_resource_group.example.name
  orchestration_platform = "DCOS"

  master_profile {
    count      = 1
    dns_prefix = "acctestmaster1"
  }

  linux_profile {
    admin_username = "acctestuser1"

    ssh_key {
      key_data = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCqaZoyiz1qbdOQ8xEf6uEu1cCwYowo5FHtsBhqLoDnnp7KUTEBN+L2NxRIfQ781rxV6Iq5jSav6b2Q8z5KiseOlvKA/RF2wqU0UPYqQviQhLmW6THTpmrv/YkUCuzxDpsH7DUDhZcwySLKVVe0Qm3+5N2Ta6UYH3lsDf9R9wTP2K/+vAnflKebuypNlmocIvakFWoZda18FOmsOoIVXQ8HWFNCuw9ZCunMSN62QGamCe3dL5cXlkgHYv7ekJE15IA9aOJcM7e90oeTqo+7HTcWfdu0qQqPWY5ujyMw/llas8tsXY85LFqRnr3gJ02bAscjc477+X+j/gkpFoN1QEmt terraform@demo.tld"
    }
  }

  agent_pool_profile {
    name       = "default"
    count      = 1
    dns_prefix = "acctestagent1"
    vm_size    = "Standard_F2"
  }

  diagnostics_profile {
    enabled = false
  }

  tags = {
    Environment = "Production"
  }
}
```

## Example Usage (Kubernetes)

```hcl
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West US"
}

resource "azurerm_container_service" "example" {
  name                   = "acctestcontservice1"
  location               = azurerm_resource_group.example.location
  resource_group_name    = azurerm_resource_group.example.name
  orchestration_platform = "Kubernetes"

  master_profile {
    count      = 1
    dns_prefix = "acctestmaster1"
  }

  linux_profile {
    admin_username = "acctestuser1"

    ssh_key {
      key_data = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCqaZoyiz1qbdOQ8xEf6uEu1cCwYowo5FHtsBhqLoDnnp7KUTEBN+L2NxRIfQ781rxV6Iq5jSav6b2Q8z5KiseOlvKA/RF2wqU0UPYqQviQhLmW6THTpmrv/YkUCuzxDpsH7DUDhZcwySLKVVe0Qm3+5N2Ta6UYH3lsDf9R9wTP2K/+vAnflKebuypNlmocIvakFWoZda18FOmsOoIVXQ8HWFNCuw9ZCunMSN62QGamCe3dL5cXlkgHYv7ekJE15IA9aOJcM7e90oeTqo+7HTcWfdu0qQqPWY5ujyMw/llas8tsXY85LFqRnr3gJ02bAscjc477+X+j/gkpFoN1QEmt terraform@demo.tld"
    }
  }

  agent_pool_profile {
    name       = "default"
    count      = 1
    dns_prefix = "acctestagent1"
    vm_size    = "Standard_F2"
  }

  service_principal {
    client_id     = "00000000-0000-0000-0000-000000000000"
    client_secret = "00000000000000000000000000000000"
  }

  diagnostics_profile {
    enabled = false
  }

  tags = {
    Environment = "Production"
  }
}
```

## Example Usage (Swarm)

```hcl
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West US"
}

resource "azurerm_container_service" "example" {
  name                   = "acctestcontservice1"
  location               = azurerm_resource_group.example.location
  resource_group_name    = azurerm_resource_group.example.name
  orchestration_platform = "Swarm"

  master_profile {
    count      = 1
    dns_prefix = "acctestmaster1"
  }

  linux_profile {
    admin_username = "acctestuser1"

    ssh_key {
      key_data = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCqaZoyiz1qbdOQ8xEf6uEu1cCwYowo5FHtsBhqLoDnnp7KUTEBN+L2NxRIfQ781rxV6Iq5jSav6b2Q8z5KiseOlvKA/RF2wqU0UPYqQviQhLmW6THTpmrv/YkUCuzxDpsH7DUDhZcwySLKVVe0Qm3+5N2Ta6UYH3lsDf9R9wTP2K/+vAnflKebuypNlmocIvakFWoZda18FOmsOoIVXQ8HWFNCuw9ZCunMSN62QGamCe3dL5cXlkgHYv7ekJE15IA9aOJcM7e90oeTqo+7HTcWfdu0qQqPWY5ujyMw/llas8tsXY85LFqRnr3gJ02bAscjc477+X+j/gkpFoN1QEmt terraform@demo.tld"
    }
  }

  agent_pool_profile {
    name       = "default"
    count      = 1
    dns_prefix = "acctestagent1"
    vm_size    = "Standard_F2"
  }

  diagnostics_profile {
    enabled = false
  }

  tags = {
    Environment = "Production"
  }
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the Container Service instance to create. Changing this forces a new resource to be created.

* `location` - (Required) The location where the Container Service instance should be created. Changing this forces a new resource to be created.

* `resource_group_name` - (Required) Specifies the resource group where the resource exists. Changing this forces a new resource to be created.

* `orchestration_platform` - (Required) Specifies the Container Orchestration Platform to use. Currently can be either `DCOS`, `Kubernetes` or `Swarm`. Changing this forces a new resource to be created.

* `master_profile` - (Required) A Master Profile block as documented below.

* `linux_profile` - (Required) A Linux Profile block as documented below.

* `agent_pool_profile` - (Required) A Agent Pool Profile's block as documented below.

* `service_principal` - (only Required when you're using `Kubernetes` as an Orchestration Platform) A Service Principal block as documented below.

* `diagnostics_profile` - (Required) A VM Diagnostics Profile block as documented below.

* `tags` - (Optional) A mapping of tags to assign to the resource.


`master_profile` supports the following:

* `count` - (Required) Number of masters (VMs) in the container service cluster. Allowed values are 1, 3, and 5. The default value is 1.
* `dns_prefix` - (Required) The DNS Prefix to use for the Container Service master nodes.

`linux_profile` supports the following:

* `admin_username` - (Required) The Admin Username for the Cluster.
* `ssh_key` - (Required) An SSH Key block as documented below.

`ssh_key` supports the following:

* `key_data` - (Required) The Public SSH Key used to access the cluster.

`agent_pool_profile` supports the following:

* `name` - (Required) Unique name of the agent pool profile in the context of the subscription and resource group.
* `count` - (Required) Number of agents (VMs) to host docker containers. Allowed values must be in the range of 1 to 100 (inclusive). The default value is 1.
* `dns_prefix` - (Required) The DNS Prefix given to Agents in this Agent Pool.
* `vm_size` - (Required) The VM Size of each of the Agent Pool VM's (e.g. Standard_F1 / Standard_D2v2).

`service_principal` supports the following:

* `client_id` - (Required) The ID for the Service Principal.
* `client_secret` - (Required) The secret password associated with the service principal.

`diagnostics_profile` supports the following:

* `enabled` - (Required) Should VM Diagnostics be enabled for the Container Service VM's

## Attributes Reference

The following attributes are exported:

* `id` - The Container Service ID.

* `master_profile.fqdn` - FDQN for the master.

* `agent_pool_profile.fqdn` - FDQN for the agent pool.

* `diagnostics_profile.storage_uri` - The URI of the storage account where diagnostics are stored.

## Timeouts

~> **Note:** Custom Timeouts are available [as an opt-in Beta in version 1.43 & 1.44 of the Azure Provider](/docs/providers/azurerm/guides/2.0-beta.html) and will be enabled by default in version 2.0 of the Azure Provider.

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:

* `create` - (Defaults to 90 minutes) Used when creating the Container Service.
* `update` - (Defaults to 90 minutes) Used when updating the Container Service.
* `read` - (Defaults to 5 minutes) Used when retrieving the Container Service.
* `delete` - (Defaults to 90 minutes) Used when deleting the Container Service.
