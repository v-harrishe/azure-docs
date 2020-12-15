---
title: CLI - Create ASP.NET Core app from ACR
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to create an Linux ASP.NET Core app from ACR.

tags: azure-service-management

ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: "devx-track-dotnet, mvc, seodec18"
---

# Create an ASP.NET Core app in a Docker container in App Service from Azure Container Registry

This sample script creates a resource group, a Linux App Service plan, and an app. It then deploys an ASP.NET Core application using a Docker Container from the Azure Container Registry.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

You need Azure CLI version 2.0.52 or later. To find the version, run `az --version`. If you need to install or upgrade, see [Install the Azure CLI](https://docs.azure.cn/cli/install-azure-cli).

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh "Linux Azure Container Registry")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands to create a resource group, App Service app, and all related resources. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az webapp config container set`](https://docs.azure.cn/cli/webapp/config/container#az_webapp_config_container_set) | Sets the Docker container for the App Service app. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli linux acr aspnetcore -->
<!--NEW.date: 12/21/2020-->