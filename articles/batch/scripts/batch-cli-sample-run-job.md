---
title: Azure CLI Script Example - Run a Batch job
description: This script creates a Batch job and adds a series of tasks to the job. It also demonstrates how to monitor a job and its tasks.
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: devx-track-azurecli

---

# CLI example: Run a job and tasks with Azure Batch

This script creates a Batch job and adds a series of tasks to the job. It also demonstrates
how to monitor a job and its tasks. 

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- This tutorial requires version 2.0.20 or later of the Azure CLI. If using Azure local Shell, the latest version is already installed. 

## Example script

[!code-azurecli-interactive[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## Clean up deployment

Run the following command to remove the
resource group and all resources associated with it.

```azurecli
az group delete --name myResourceGroup
```

## Script explanation

This script uses the following commands. Each command in the table links to command-specific documentation.

| Command | Notes |
|---|---|
| [az group create](https://docs.azure.cn/cli/group#az_group_create) | Creates a resource group in which all resources are stored. |
| [az batch account create](https://docs.azure.cn/cli/batch/account#az_batch_account_create) | Creates the Batch account. |
| [az batch account login](https://docs.azure.cn/cli/batch/account#az_batch_account_login) | Authenticates against the specified Batch account for further CLI interaction.  |
| [az batch pool create](https://docs.azure.cn/cli/batch/pool#az_batch_pool_create) | Creates a pool of compute nodes.  |
| [az batch job create](https://docs.azure.cn/cli/batch/job#az_batch_job_create) | Creates a Batch job.  |
| [az batch task create](https://docs.azure.cn/cli/batch/task#az_batch_task_create) | Adds a task to the specified Batch job.  |
| [az batch job set](https://docs.azure.cn/cli/batch/job#az_batch_job_set) | Updates properties of a Batch job.  |
| [az batch job show](https://docs.azure.cn/cli/batch/job#az_batch_job_show) | Retrieves details of a specified Batch job.  |
| [az batch task show](https://docs.azure.cn/cli/batch/task#az_batch_task_show) | Retrieves the details of a task from the specified Batch job.  |
| [az group delete](https://docs.azure.cn/cli/group#az_group_delete) | Deletes a resource group including all nested resources. |

## Next steps

For more information on the Azure CLI, see [Azure CLI documentation](https://docs.azure.cn/cli).



<!-- Update_Description: new article about scripts/batch cli sample run job -->
<!--NEW.date: 12/21/2020-->