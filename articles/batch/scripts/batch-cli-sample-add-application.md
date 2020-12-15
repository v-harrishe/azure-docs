---
title: Azure CLI Script Example - Add an Application in Batch
description: This sample script demonstrates how to add an application for use with an Azure Batch pool or a task.
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: devx-track-azurecli

---

# CLI example: Add an application to an Azure Batch account

This script demonstrates how to add an application for use with an Azure Batch pool or task. To set up an application to add to your Batch account, package your executable, together with any dependencies, into a zip file. 

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - This tutorial requires version 2.0.20 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed. 

## Example script

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## Clean up deployment

Run the following command to remove the
resource group and all resources associated with it.

```azurecli
az group delete --name myResourceGroup
```

## Script explanation

This script uses the following commands.
Each command in the table links to command-specific documentation.

| Command | Notes |
|---|---|
| [az group create](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [az storage account create](https://docs.azure.cn/cli/storage/account#az_storage_account_create) | Creates a storage account. |
| [az batch account create](https://docs.azure.cn/cli/batch/account#az_batch_account_create) | Creates the Batch account. |
| [az batch account login](https://docs.azure.cn/cli/batch/account#az_batch_account_login) | Authenticates against the specified Batch account for further CLI interaction.  |
| [az batch application create](https://docs.azure.cn/cli/batch/application#az_batch_application_create) | Creates an application.  |
| [az batch application package create](https://docs.azure.cn/cli/batch/application/package#az_batch_application_package_create) | Adds an application package to the specified application.  |
| [az batch application set](https://docs.azure.cn/cli/batch/application#az_batch_application_set) | Updates properties of an application.  |
| [az group delete](https://docs.azure.cn/cli/group#az_group_delete) | Deletes a resource group including all nested resources. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).



<!-- Update_Description: new article about scripts/batch cli sample add application -->
<!--NEW.date: 12/21/2020-->