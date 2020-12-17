---
title: CLI - Connect an app to a storage account
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to connect an app to a storage account.

tags: azure-service-management

ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: mvc, seodec18
---

# Connect an App Service app to a storage account using CLI

This sample script creates an Azure storage account and an App Service app. It then links the storage account to the app using app settings.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

If you choose to install and use the CLI locally, you need Azure CLI version 2.0 or later. To find the version, run `az --version`. If you need to install or upgrade, see [Install the Azure CLI](https://docs.azure.cn/cli/install-azure-cli).


## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands to create a resource group, App Service app, storage account, and all related resources. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az storage account create`](https://docs.azure.cn/cli/storage/account#az_storage_account_create) | Creates a storage account. |
| [`az storage account show-connection-string`](https://docs.azure.cn/cli/storage/account#az_storage_account_show_connection_string) | Get the connection string for a storage account. |
| [`az webapp config appsettings set`](https://docs.azure.cn/cli/webapp/config/appsettings#az_webapp_config_appsettings_set) | Creates or updates an app setting for an App Service app. App settings are exposed as environment variables for your app. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli connect to storage -->
<!--NEW.date: 12/21/2020-->