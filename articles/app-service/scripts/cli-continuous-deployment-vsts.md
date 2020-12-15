---
title: 'Continuous deployment from Azure Repos'
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to set up CI/CD from Azure Repos.

tags: azure-service-management

ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: mvc, seodec18, devx-track-azurecli
---
# Create an App Service app with continuous deployment using Azure CLI

This sample script creates an app in App Service with its related resources, and then sets up continuous deployment from an Azure DevOps repository. For this sample, you need:

* An Azure DevOps repository with application code, that you have administrative permissions for.
* A [Personal Access Token (PAT)](https://docs.azure.cn/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts) for your Azure DevOps organization.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - This tutorial requires version 2.0 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create an app with continuous deployment from Azure DevOps")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az webapp deployment source config`](https://docs.azure.cn/cli/webapp/deployment/source#az_webapp_deployment_source_config) | Associates an App Service app with a Git or Mercurial repository. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli continuous deployment vsts -->
<!--NEW.date: 12/21/2020-->