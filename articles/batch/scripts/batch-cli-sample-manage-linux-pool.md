---
title: Azure CLI Script Example - Linux Pool in Batch
description: This script demonstrates some of the commands available in the Azure CLI to create and manage a pool of Linux compute nodes in Azure Batch.
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: devx-track-azurecli

---

# CLI example: Create and manage a Linux pool in Azure Batch

This script demonstrates some of the commands available in the Azure CLI to create and manage a pool of Linux compute nodes in Azure Batch.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- This tutorial requires version 2.0.20 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed. 

## Example script

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Linux Virtual Machine Pool")]

## Clean up deployment

Run the following command to remove the
resource group and all resources associated with it.

```azurecli
az group delete --name myResourceGroup
```

## Script explanation

This script uses the following commands. Each command in the table links to command-specific documentation.

| Command | Notes |
|---|---|
| [az group create](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [az batch account create](https://docs.azure.cn/cli/batch/account#az_batch_account_create) | Creates the Batch account. |
| [az batch account login](https://docs.azure.cn/cli/batch/account#az_batch_account_login) | Authenticates against the specified Batch account for further CLI interaction.  |
| [az batch pool node-agent-skus list](../batch-linux-nodes.md#list-of-virtual-machine-images) | Lists available node agent SKUs and image information.  |
| [az batch pool create](https://docs.azure.cn/cli/batch/pool#az_batch_pool_create) | Creates a pool of compute nodes.  |
| [az batch pool resize](https://docs.azure.cn/cli/batch/pool#az_batch_pool_resize) | Resizes the number of running VMs in the specified pool.  |
| [az batch pool show](https://docs.azure.cn/cli/batch/pool#az_batch_pool_show) | Displays the properties of a pool.  |
| [az batch node list](https://docs.azure.cn/cli/batch/node#az_batch_node_list) | Lists all the compute node in the specified pool.  |
| [az batch node reboot](https://docs.azure.cn/cli/batch/node#az_batch_node_reboot) | Reboots the specified compute node.  |
| [az batch node delete](https://docs.azure.cn/cli/batch/node#az_batch_node_delete) | Deletes the listed nodes from the specified pool.  |
| [az group delete](https://docs.azure.cn/cli/group#az_group_delete) | Deletes a resource group including all nested resources. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).



<!-- Update_Description: new article about scripts/batch cli sample manage linux pool -->
<!--NEW.date: 12/21/2020-->