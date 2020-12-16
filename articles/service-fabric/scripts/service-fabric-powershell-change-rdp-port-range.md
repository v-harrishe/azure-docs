---
title: Azure PowerShell Script Sample - Change the RDP port range | Azure Docs
description: Azure PowerShell Script Sample - Changes the RDP port range of a deployed cluster.
services: service-fabric
tags: azure-service-management


ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
---

# Update the RDP port range values

This sample script changes the RDP port range values on the cluster node VMs after the cluster has been deployed.  Azure PowerShell is used so that the underlying VMs do not cycle.  The script gets the `Microsoft.Network/loadBalancers` resource in the cluster's resource group and updates the `inboundNatPools.frontendPortRangeStart` and `inboundNatPools.frontendPortRangeEnd` values. Customize the parameters as needed.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azure/).

## Sample script

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## Script explanation

This script uses the following commands. Each command in the table links to command specific documentation.

| Command | Notes |
|---|---|
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Gets the `Microsoft.Network/loadBalancers` resource. |
|[Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource)|Updates the `Microsoft.Network/loadBalancers` resource.|

## Next steps

For more information on the Azure PowerShell module, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/).

Additional Azure Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).



<!-- Update_Description: new article about service fabric powershell change rdp port range -->
<!--NEW.date: 12/21/2020-->