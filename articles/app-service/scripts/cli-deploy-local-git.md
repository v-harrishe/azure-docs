---
title: CLI - Deploy from local Git repo
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to deploy code from a local Git repository.

tags: azure-service-management

ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: mvc, seodec18, devx-track-azurecli
---

# Create an App Service app and deploy code from a local Git repository using Azure CLI

This sample script creates an app in App Service with its related resources, and then deploys your app code in a local Git repository.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - This tutorial requires version 2.0 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create an app and deploy code from a local Git repository")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az webapp deployment user set`](https://docs.azure.cn/cli/webapp/deployment/user#az_webapp_deployment_user_set) | Sets the account-level deployment credentials for App Service. |
| [`az webapp deployment source config-local-git`](https://docs.azure.cn/cli/webapp/deployment/source#az_webapp_deployment_source_config_local_git) | Creates a source control configuration for a local Git repository. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli deploy local git -->
<!--NEW.date: 12/21/2020-->