---
title: CLI - Deploy to staging slot
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to deploy code to a staging slot.

tags: azure-service-management

ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: mvc, seodec18, devx-track-azurecli
---

# Create an App Service app and deploy code to a staging environment using Azure CLI

This sample script creates an app in App Service with an additional deployment slot called "staging", and then deploys a sample app to the "staging" slot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - This tutorial requires version 2.0 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create an app and deploy code to a staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az webapp deployment slot create`](https://docs.azure.cn/cli/webapp/deployment/slot#az_webapp_deployment_slot_create) | Create a deployment slot. |
| [`az webapp deployment source config`](https://docs.azure.cn/cli/webapp/deployment/source#az_webapp_deployment_source_config) | Associates an App Service app with a Git or Mercurial repository. |
| [`az webapp deployment slot swap`](https://docs.azure.cn/cli/webapp/deployment/slot#az_webapp_deployment_slot_swap) | Swap a specified deployment slot into production. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli deploy staging environment -->
<!--NEW.date: 12/21/2020-->