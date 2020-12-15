---
title: Quickstart - Create an internal load balancer by using a template
description: This quickstart shows how to create an internal Azure load balancer by using an Azure Resource Manager template (ARM template).
services: load-balancer

ms.service: load-balancer
ms.topic: quickstart
ms.custom: subject-armqs, devx-track-azurecli

author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
---

# Quickstart: Create an internal load balancer to load balance VMs by using an ARM template

This quickstart describes how to use an Azure Resource Manager template (ARM template) to create an internal Azure load balancer.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

If your environment meets the prerequisites and you're familiar with using ARM templates, select the **Deploy to Azure** button. The template will open in the Azure portal.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Deploy to Azure":::](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-2-vms-internal-load-balancer%2Fazuredeploy.json)

## Prerequisites

If you don't have an Azure subscription, create a [trial account](https://www.azure.cn/pricing/1rmb-trial-full/) before you begin.

## Review the template

The template used in this quickstart is from the [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/201-2-vms-internal-load-balancer).

:::code language="json" source="~/quickstart-templates/201-2-vms-internal-load-balancer/azuredeploy.json":::

Multiple Azure resources have been defined in the template:

- [**Microsoft.Storage/storageAccounts**](https://docs.azure.cn/azure/templates/microsoft.storage/storageaccounts): Virtual machine storage accounts for boot diagnostics.
- [**Microsoft.Compute/availabilitySets**](https://docs.azure.cn/azure/templates/microsoft.compute/availabilitySets): Availability set for virtual machines.
- [**Microsoft.Network/virtualNetworks**](https://docs.azure.cn/azure/templates/microsoft.network/virtualNetworks): Virtual network for load balancer and virtual machines.
- [**Microsoft.Network/networkInterfaces**](https://docs.azure.cn/azure/templates/microsoft.network/networkInterfaces): Network interfaces for virtual machines.
- [**Microsoft.Network/loadBalancers**](https://docs.azure.cn/azure/templates/microsoft.network/loadBalancers): Internal load balancer.
- [**Microsoft.Compute/virtualMachines**](https://docs.azure.cn/azure/templates/microsoft.compute/virtualMachines): Virtual machines.

To find more templates that are related to Azure Load Balancer, see [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## Deploy the template

**Azure CLI**

```azurecli
read -p "Enter the location (i.e. chinanorth): " location
resourceGroupName="myResourceGroupLB"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json"

az group create \
--name $resourceGroupName \
--location $location

az deployment group create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

## Review deployed resources

1. Sign in to the [Azure portal](https://portal.azure.cn).

1. Select **Resource groups** from the left pane.

1. Select the resource group that you created in the previous section. The default resource group name is **myResourceGroupLB**

1. Verify the following resources were created in the resource group:

:::image type="content" source="media/quickstart-load-balancer-standard-internal-template/verify-creation.png" alt-text="User Azure portal to verify creation of resources." border="true":::

## Clean up resources

When no longer needed, you can use the [az group delete](https://docs.azure.cn/cli/group#az_group_delete) command to remove the resource group and all resources contained within.

```azurecli
  az group delete \
    --name myResourceGroupLB
```

## Next steps

For a step-by-step tutorial that guides you through the process of creating a template, see:

> [!div class="nextstepaction"]
> [Tutorial: Create and deploy your first ARM template](../azure-resource-manager/templates/template-tutorial-create-first-template.md)



<!-- Update_Description: new article about quickstart load balancer standard internal template -->
<!--NEW.date: 12/21/2020-->