---
title: Use Azure PowerShell to create a Service Bus queue
description: In this quickstart, you learn how to create a Service Bus namespace and a queue in the namespace by using the Azure PowerShell.  

ms.devlang: dotnet
ms.topic: quickstart
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: devx-track-azurepowershell
---

# Use Azure PowerShell to create a Service Bus namespace and a queue
This quickstart shows you how to create a Service Bus namespace and a queue using the Azure PowerShell. It also shows you how to get authorization credentials that a client application can use to send/receive messages to/from the queue. 

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]


## Prerequisites

To complete this quickstart, make sure you have an Azure subscription. If you don't have an Azure subscription, you can create a [trial account][] before you begin. 

In this quickstart, you use Azure local Shell that you can launch after sign into the Azure portal. For details about Azure local Shell, see [Overview of Azure local Shell](../cloud-shell/overview.md). You can also [install](https://docs.microsoft.com/powershell/azure/install-Az-ps) and use Azure PowerShell on your machine. 


## Provision resources
1. Sign into the [Azure portal](https://portal.azure.cn).
2. Launch Azure local Shell by selecting the icon shown in the following image: 

    :::image type="content" source="./media/service-bus-quickstart-powershell/launch-cloud-shell.png" alt-text="Launch local Shell":::
3. In the bottom local Shell window, switch from **Bash** to **PowerShell**. 

    :::image type="content" source="./media/service-bus-quickstart-powershell/cloud-power-shell.png" alt-text="Switch to PowerShell mode":::    
4. Run the following command to create an Azure resource group. Update the resource group name and the location if you want. 

    ```powershell
    New-AzResourceGroup –Name ContosoRG –Location chinaeast
    ```
5. Run the following command to create a Service Bus messaging namespace. In this example, `ContosoRG` is the resource group you created in the previous step. `ContosoSBusNS` is the name of the Service Bus namespace created in that resource group. 

    ```powershell
    New-AzServiceBusNamespace -ResourceGroupName ContosoRG -Name ContosoSBusNS -Location chinaeast
    ```
6. Run the following to create a queue in the namespace you created in the previous step. 

    ```powershell
    New-AzServiceBusQueue -ResourceGroupName ContosoRG -NamespaceName ContosoSBusNS -Name ContosoOrdersQueue 
    ```
7. Get the primary connection string for the namespace. You use this connection string to connect to the queue and send and receive messages. 

    ```powershell    
    Get-AzServiceBusKey -ResourceGroupName ContosoRG -Namespace ContosoSBusNS -Name RootManageSharedAccessKey
    ```

    Note down the connection string and the queue name. You use them to send and receive messages. 


## Next steps
In this article, you created a Service Bus namespace and a queue in the namespace. To learn how to send/receive messages to/from the queue, see one of the following quickstarts in the **Send and receive messages** section. 

- [.NET](service-bus-dotnet-get-started-with-queues.md)
- [Java](service-bus-java-how-to-use-queues.md)
- [JavaScript](service-bus-nodejs-how-to-use-queues.md)
- [Python](service-bus-python-how-to-use-queues.md)
- [PHP](service-bus-php-how-to-use-queues.md)
- [Ruby](service-bus-ruby-how-to-use-queues.md)

[trial account]: https://www.azure.cn/free/




<!-- Update_Description: new article about service bus quickstart powershell -->
<!--NEW.date: 12/21/2020-->