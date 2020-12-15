---
title: Azure CLI script sample - Integrate App Service with Application Gateway | Azure Docs
description: Azure CLI script sample - Integrate App Service with Application Gateway
services: appservice
documentationcenter: appservice

manager: ccompy
editor: 
tags: azure-service-management

ms.assetid: 6c16d6f8-3c08-4a59-858e-684a2ceccb5f
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: seodec18, devx-track-azurecli
---

# Integrate App Service with Application Gateway using CLI

This sample script creates an Azure App Service web app, an Azure Virtual Network and an Application Gateway. It then restricts the traffic for the web app to only originate from the Application Gateway subnet.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - This tutorial requires version 2.0.74 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/integrate-with-app-gateway/integrate-with-app-gateway.sh "Integrate with Application Gateway")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands to create a resource group, App Service app, Cosmos DB, and all related resources. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az network vnet create`](https://docs.azure.cn/cli/network/vnet#az_network_vnet_create) | Creates a virtual network. |
| [`az network public-ip create`](https://docs.azure.cn/cli/network/public-ip#az_network_public_ip_create) | Creates a public IP address. |
| [`az network public-ip show`](https://docs.azure.cn/cli/network/public-ip#az_network_public_ip_show) | Show details of a public IP address. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service web app. |
| [`az webapp show`](https://docs.azure.cn/cli/webapp#az_webapp_show) | Show details of an App Service web app. |
| [`az webapp config access-restriction add`](https://docs.azure.cn/cli/webapp/config/access-restriction#az_webapp_config_access_restriction_add) | Adds an access restriction to the App Service web app. |
| [`az network application-gateway create`](https://docs.azure.cn/cli/network/application-gateway#az_network_application_gateway_create) | Creates an Application Gateway. |
| [`az network application-gateway http-settings update`](https://docs.azure.cn/cli/network/application-gateway/http-settings#az_network_application_gateway_http_settings_update) | Updates Application Gateway HTTP settings. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli integrate app service with application gateway -->
<!--NEW.date: 12/21/2020-->