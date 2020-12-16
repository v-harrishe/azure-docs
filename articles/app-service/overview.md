---
title: Overview
description: Learn how Azure App Service helps you develop and host web applications

ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.topic: overview
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.custom: "devx-track-dotnet, mvc, seodec18"
---

# App Service overview

*Azure App Service* is an HTTP-based service for hosting web applications, REST APIs, and mobile back ends. You can develop in your favorite language, be it .NET, .NET Core, Java, Ruby, Node.js, PHP, or Python. Applications run and scale with ease on both Windows and [Linux](#app-service-on-linux)-based environments.

App Service not only adds the power of Azure Azure to your application, such as security, load balancing, autoscaling, and automated management. You can also take advantage of its DevOps capabilities, such as continuous deployment from Azure DevOps, GitHub, Docker Hub, and other sources, package management, staging environments, custom domain, and TLS/SSL certificates. 

With App Service, you pay for the Azure compute resources you use. The compute resources you use are determined by the _App Service plan_ that you run your apps on. For more information, see [Azure App Service plans overview](overview-hosting-plans.md).

## Why use App Service?

Here are some key features of App Service:

* **Multiple languages and frameworks** - App Service has first-class support for ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP, or Python. You can also run [PowerShell and other scripts or executables](webjobs-create.md) as background services.
* **Managed production environment** - App Service automatically [patches and maintains the OS and language frameworks](overview-patch-os-runtime.md) for you. Spend time writing great apps and let Azure worry about the platform.
* **Containerization and Docker** - Dockerize your app and host a custom Windows or Linux container in App Service. Run multi-container apps with Docker Compose. Migrate your Docker skills directly to App Service.
* **DevOps optimization** - Set up [continuous integration and deployment](deploy-continuous-deployment.md) with Azure DevOps, GitHub, BitBucket, Docker Hub, or Azure Container Registry. Promote updates through [test and staging environments](deploy-staging-slots.md). Manage your apps in App Service by using [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) or the [cross-platform command-line interface (CLI)](https://docs.azure.cn/cli/install-azure-cli).
* **Global scale with high availability** - Scale [up](manage-scale-up.md) or [out](../azure-monitor/platform/autoscale-get-started.md) manually or automatically. Host your apps anywhere in Azure's global datacenter infrastructure, and the App Service [SLA](https://www.azure.cn/support/legal/sla/app-service/) promises high availability.
* **Connections to SaaS platforms and on-premises data** - Choose from more than 50 [connectors](../connectors/apis-list.md) for enterprise systems (such as SAP), SaaS services (such as Salesforce), and internet services (such as Facebook). Access on-premises data using [Hybrid Connections](app-service-hybrid-connections.md) and [Azure Virtual Networks](web-sites-integrate-with-vnet.md).
* **Security and compliance** - App Service is [ISO, SOC, and PCI compliant](https://www.microsoft.com/en-us/trustcenter). Authenticate users with [Azure Active Directory](configure-authentication-provider-aad.md), [Google](configure-authentication-provider-google.md), [Facebook](configure-authentication-provider-facebook.md), [Twitter](configure-authentication-provider-twitter.md), or [Microsoft account](configure-authentication-provider-microsoft.md). Create [IP address restrictions](app-service-ip-restrictions.md) and [manage service identities](overview-managed-identity.md).
* **Application templates** - Choose from an extensive list of application templates in the [Azure Marketplace](https://market.azure.cn/marketplace/), such as WordPress, Joomla, and Drupal.
* **Visual Studio and Visual Studio Code integration** - Dedicated tools in Visual Studio and Visual Studio Code streamline the work of creating, deploying, and debugging.
* **API and mobile features** - App Service provides turn-key CORS support for RESTful API scenarios, and simplifies mobile app scenarios by enabling authentication, offline data sync, push notifications, and more.
* **Serverless code** - Run a code snippet or script on-demand without having to explicitly provision or manage infrastructure, and pay only for the compute time your code actually uses (see [Azure Functions](../azure-functions/index.yml)).

Besides App Service, Azure offers other services that can be used for hosting websites and web applications. For most scenarios, App Service is the best choice.  For microservice architecture, consider [Azure Spring-Cloud Service](../spring-cloud/index.yml) or [Service Fabric](https://www.azure.cn/documentation/services/service-fabric).  If you need more control over the VMs on which your code runs, consider [Azure Virtual Machines](https://www.azure.cn/documentation/services/virtual-machines/). For more information about how to choose between these Azure services, see [Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](https://docs.azure.cn/azure/architecture/guide/technology-choices/compute-decision-tree).

## App Service on Linux

App Service can also host web apps natively on Linux for supported application stacks. It can also run custom Linux containers (also known as Web App for Containers).

### Built-in languages and frameworks

App Service on Linux supports a number of language specific built-in images. Just deploy your code. Supported languages include: Node.js, Java (JRE 8 & JRE 11), PHP, Python, .NET Core and Ruby. Run [`az webapp list-runtimes --linux`](https://docs.azure.cn/cli/webapp#az_webapp_list_runtimes) to view the latest languages and supported versions. If the runtime your application requires is not supported in the built-in images, you can deploy it with a custom container.

### Limitations

- App Service on Linux is not supported on [Shared](https://www.azure.cn/pricing/details/app-service/plans/) pricing tier. 
- You can't mix Windows and Linux apps in the same App Service plan.  
- Within the same resource group, you can't mix Windows and Linux apps in the same region.
- The Azure portal shows only features that currently work for Linux apps. As features are enabled, they're activated on the portal.
- When deployed to built-in images, your code and content are allocated a storage volume for web content, backed by Azure Storage. The disk latency of this volume is higher and more variable than the latency of the container filesystem. Apps that require heavy read-only access to content files may benefit from the custom container option, which places files in the container filesystem instead of on the content volume.

## Next steps

Create your first web app.

> [!div class="nextstepaction"]
> [ASP.NET Core (on Windows or Linux)](quickstart-dotnetcore.md)

> [!div class="nextstepaction"]
> [ASP.NET (on Windows)](quickstart-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP (on Windows or Linux)](quickstart-php.md)

> [!div class="nextstepaction"]
> [Ruby (on Linux)](quickstart-ruby.md)

> [!div class="nextstepaction"]
> [Node.js (on Windows or Linux)](quickstart-nodejs.md)

> [!div class="nextstepaction"]
> [Java (on Windows or Linux)](quickstart-java.md)

> [!div class="nextstepaction"]
> [Python (on Linux)](quickstart-python.md)

> [!div class="nextstepaction"]
> [HTML (on Windows or Linux)](quickstart-html.md)

> [!div class="nextstepaction"]
> [Custom container (Windows or Linux)](tutorial-custom-container.md)



<!-- Update_Description: new article about overview -->
<!--NEW.date: 12/21/2020-->