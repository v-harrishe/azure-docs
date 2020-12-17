---
title: CLI - Upload and bind TLS/SSL cert to an app
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to bind a custom TLS/SSL certificate to an app.
tags: azure-service-management

ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: mvc, seodec18, devx-track-azurecli
---

# Bind a custom TLS/SSL certificate to an App Service app using CLI

This sample script creates an app in App Service with its related resources, then binds the TLS/SSL certificate of a custom domain name to it. For this sample, you need:

* Access to your domain registrar's DNS configuration page.
* A valid .PFX file and its password for the TLS/SSL certificate you want to upload and bind.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

If you choose to install and use the CLI locally, you need Azure CLI version 2.0 or later. To find the version, run `az --version`. If you need to install or upgrade, see [Install the Azure CLI](https://docs.azure.cn/cli/install-azure-cli).

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom TLS/SSL certificate to an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az group create`](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [`az appservice plan create`](https://docs.azure.cn/cli/appservice/plan#az_appservice_plan_create) | Creates an App Service plan. |
| [`az webapp create`](https://docs.azure.cn/cli/webapp#az_webapp_create) | Creates an App Service app. |
| [`az webapp config hostname add`](https://docs.azure.cn/cli/webapp/config/hostname#az_webapp_config_hostname_add) | Maps a custom domain to an App Service app. |
| [`az webapp config ssl upload`](https://docs.azure.cn/cli/webapp/config/ssl#az_webapp_config_ssl_upload) | Uploads a TLS/SSL certificate to an App Service app. |
| [`az webapp config ssl bind`](https://docs.azure.cn/cli/webapp/config/ssl#az_webapp_config_ssl_bind) | Binds an uploaded TLS/SSL certificate to an App Service app. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli configure ssl certificate -->
<!--NEW.date: 12/21/2020-->