---
title: Use the Azure portal to create a Service Bus queue
description: In this quickstart, you learn how to create a Service Bus namespace and a queue in the namespace by using the Azure portal. 

ms.topic: quickstart
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
---

# Use Azure portal to create a Service Bus namespace and a queue
This quickstart shows you how to create a Service Bus namespace and a queue using the [Azure portal][Azure portal]. It also shows you how to get authorization credentials that a client application can use to send/receive messages to/from the queue. 

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Prerequisites

To complete this quickstart, make sure you have an Azure subscription. If you don't have an Azure subscription, you can create a [trial account][] before you begin.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## Next steps
In this article, you created a Service Bus namespace and a queue in the namespace. To learn how to send/receive messages to/from the queue, see one of the following quickstarts in the **Send and receive messages** section. 

- [.NET](service-bus-dotnet-get-started-with-queues.md)
- [Java](service-bus-java-how-to-use-queues.md)
- [JavaScript](service-bus-nodejs-how-to-use-queues.md)
- [Python](service-bus-python-how-to-use-queues.md)
- [PHP](service-bus-php-how-to-use-queues.md)
- [Ruby](service-bus-ruby-how-to-use-queues.md)

[trial account]: https://www.azure.cn/free/
[Azure portal]: https://portal.azure.cn/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png



<!-- Update_Description: new article about service bus quickstart portal -->
<!--NEW.date: 12/21/2020-->