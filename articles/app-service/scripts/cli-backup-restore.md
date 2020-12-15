---
title: CLI - Restore an app from a backup
description: Learn how to use the Azure CLI to automate deployment and management of your App Service app. This sample shows how to restore an app from a backup.

tags: azure-service-management

ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.reviewer: cephalin
ms.custom: mvc, seodec18
---

# Restore a web app from a backup using CLI

This sample script creates a web app in App Service with its related resources, and then creates a one-time backup for it. 

To run this script, you need an existing backup for a web app. To create one, see [Backup up a web app](cli-backup-onetime.md) or [Create a scheduled backup for a web app](cli-backup-scheduled.md).

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

If you choose to install and use the CLI locally, you need Azure CLI version 2.0 or later. To find the version, run `az --version`. If you need to install or upgrade, see [Install the Azure CLI](https://docs.azure.cn/cli/install-azure-cli). 

## Sample script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-restore/backup-restore.sh?highlight=3-4,9 "Restore a web app from a backup")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [`az webapp config backup list`](https://docs.azure.cn/cli/webapp/config/backup#az_webapp_config_backup_list) | Gets a list of backups for a web app. |
| [`az webapp config backup restore`](https://docs.azure.cn/cli/webapp/config/backup#az_webapp_config_backup_restore) | Restores a web app from a backup. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Service CLI script samples can be found in the [Azure App Service documentation](../samples-cli.md).



<!-- Update_Description: new article about cli backup restore -->
<!--NEW.date: 12/21/2020-->