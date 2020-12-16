---
title: Import or export data with Azure App Configuration
description: Learn how to import or export configuration data to or from Azure App Configuration. Exchange data between your App Configuration store and code project.
services: azure-app-configuration

ms.service: azure-app-configuration
ms.topic: conceptual
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
---

# Import or export configuration data

Azure App Configuration supports data import and export operations. Use these operations to work with configuration data in bulk and exchange data between your App Configuration store and code project. For example, you can set up one App Configuration store for testing and another for production. You can copy application settings between them so that you don't have to enter data twice.

This article provides a guide for importing and exporting data with App Configuration. If you’d like to set up an ongoing sync with your GitHub repo, take a look at our [GitHub Action](./concept-github-action.md).

## Import data

Import brings configuration data into an App Configuration store from an existing source. Use the import function to migrate data into an App Configuration store or aggregate data from multiple sources. App Configuration supports importing from a JSON, YAML, or properties file.

Import data by using either the [Azure portal](https://portal.azure.cn) or the [Azure CLI](./scripts/cli-import.md). From the Azure portal, follow these steps:

1. Browse to your App Configuration store, and select **Import/Export** from the **Operations** menu.

1. On the **Import** tab, select **Source service** > **Configuration File**.

1. Select **For language** and select your desired input type.

1. Select the **Folder** icon, and browse to the file to import.

    :::image type="content" source="./media/import-file.png" alt-text="Import file":::

1. Select a **Separator**, and optionally enter a **Prefix** to use for imported key names.

1. Optionally, select a **Label**.

1. Select **Apply** to finish the import.

    :::image type="content" source="./media/import-file-complete.png" alt-text="Import file finished":::

## Export data

Export writes configuration data stored in App Configuration to another destination. Use the export function, for example, to save data in an App Configuration store to a file that's embedded with your application code during deployment.

Export data by using either the [Azure portal](https://portal.azure.cn) or the [Azure CLI](./scripts/cli-export.md). From the Azure portal, follow these steps:

1. Browse to your App Configuration store, and select **Import/Export**.

1. On the **Export** tab, select **Target service** > **Configuration File**.

1. Optionally enter a **Prefix** and select a **Label** and a point-in-time for keys to be exported.

1. Select a **File type** > **Separator**.

1. Select **Apply** to finish the export.

    :::image type="content" source="./media/export-file-complete.png" alt-text="Export file finished":::

## Next steps

> [!div class="nextstepaction"]
> [Create an ASP.NET Core web app](./quickstart-aspnet-core-app.md)


<!-- Update_Description: new article about howto import export data -->
<!--NEW.date: 12/21/2020-->