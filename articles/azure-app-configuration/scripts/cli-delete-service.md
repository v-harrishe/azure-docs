---
title: Azure CLI Script Sample - Delete an Azure App Configuration Store
titleSuffix: Azure App Configuration
description: Delete an Azure App Configuration store using a sample Azure CLI script. See reference article links to commands used in the script.
services: azure-app-configuration


ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: devx-track-azurecli
---

# Delete an Azure App Configuration store

This sample script deletes an instance of Azure App Configuration.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - This tutorial requires version 2.0 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed.

## Sample script

```azurecli
#/bin/bash

# Delete an App Configuration store named myTestAppConfigStore from the Resource Group myResourceGroup
az appconfig delete --name myTestAppConfigStore --resource-group myResourceGroup
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## Script explanation

This script uses the following commands to delete an App Configuration store. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [az appconfig delete](https://docs.azure.cn/cli/appconfig#az_appconfig_delete) | Deletes an App Configuration store resource. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).

Additional App Configuration CLI script samples can be found in the [Azure App Configuration CLI samples](../cli-samples.md).



<!-- Update_Description: new article about cli delete service -->
<!--NEW.date: 12/21/2020-->