---
title: CLI - Create a scheduled backup
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to create a scheduled backup for an app.

tags: azure-service-management

ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.reviewer: cephalin
ms.custom: mvc, seodec18, devx-track-azurecli
---

# Create a scheduled backup for an App Service app using CLI

This sample script creates an app in App Service with its related resources, and then creates a scheduled backup for it. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

If you choose to install and use the CLI locally, you need Azure CLI version 2.0 or later. To find the version, run `az --version`. If you need to install or upgrade, see [Install the Azure CLI](https://docs.azure.cn/cli/install-azure-cli). 

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-scheduled/backup-scheduled.sh?highlight=3-7 "Create a scheduled backup for an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az storage account create`](https://docs.azure.cn/cli/storage/account#az_storage_account_create) | Creates a storage account. |
| [`az storage container create`](https://docs.azure.cn/cli/storage/container#az_storage_container_create) | Creates an Azure storage container. |
| [`az storage container generate-sas`](https://docs.azure.cn/cli/storage/container#az_storage_container_generate_sas) | Generates an SAS token for an Azure storage container.  |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az webapp config backup update`](https://docs.azure.cn/cli/webapp/config/backup#az_webapp_config_backup_update) | Configures a new backup schedule for an App Service app. |
| [`az webapp config backup show`](https://docs.azure.cn/cli/webapp/config/backup#az_webapp_config_backup_show) | Shows the backup schedule for an App Service app. |
| [`az webapp config backup list`](https://docs.azure.cn/cli/webapp/config/backup#az_webapp_config_backup_list) | Gets a list of backups for an App Service app. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli backup scheduled -->
<!--NEW.date: 12/21/2020-->