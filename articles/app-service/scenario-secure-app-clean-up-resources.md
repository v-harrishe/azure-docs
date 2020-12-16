---
title: Tutorial - Clean up resources | Azure
description: In this tutorial, you learn how to clean up the Azure resources allocated while creating the web app.
services: storage, app-service-web

manager: CelesteDG

ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
ms.reviewer: stsoneff
ms.custom: azureday1
#Customer intent: As an application developer, I want to learn how to access Azure Storage for an app using managed identities.
---

# Tutorial: Clean up resources

If you completed all the steps in this multipart tutorial, you created an app service, app service hosting plan, and a storage account in a resource group. You also created an app registration in Azure Active Directory. When no longer needed, delete these resources and app registration so that you don't continue to accrue charges.

In this tutorial, you learn how to:

> [!div class="checklist"]
>
> * Delete the Azure resources created while following the tutorial.

## Delete the resource group

In the [Azure portal](https://portal.azure.cn), select **Resource groups** from the portal menu and select the resource group that contains your app service and app service plan.

Select **Delete resource group** to delete the resource group and all the resources.

:::image type="content" alt-text="Screenshot that shows deleting the resource group." source="./media/scenario-secure-app-clean-up-resources/delete-resource-group.png":::

This command might take several minutes to run.

## Delete the app registration

From the portal menu, select **Azure Active Directory** > **App registrations**. Then select the application you created.
:::image type="content" alt-text="Screenshot that shows selecting app registration." source="./media/scenario-secure-app-clean-up-resources/select-app-registration.png":::

In the app registration overview, select **Delete**.
:::image type="content" alt-text="Screenshot that shows deleting the app registration." source="./media/scenario-secure-app-clean-up-resources/delete-app-registration.png":::

## Next steps

In this tutorial, you learned how to:

> [!div class="checklist"]
>
> * Delete the Azure resources created while following the tutorial.

Learn how to connect a [.NET Core app](tutorial-dotnetcore-sqldb-app.md), [Python app](tutorial-python-postgresql-app.md), [Java app](tutorial-java-spring-cosmosdb.md), or [Node.js app](tutorial-nodejs-mongodb-app.md) to a database.


<!-- Update_Description: new article about scenario secure app clean up resources -->
<!--NEW.date: 12/21/2020-->