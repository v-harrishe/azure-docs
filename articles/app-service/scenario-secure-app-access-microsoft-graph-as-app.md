---
title: Tutorial - Web app accesses Azure Graph as the app| Azure
description: In this tutorial, you learn how to access data in Azure Graph by using managed identities.
services: microsoft-graph, app-service-web

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
#Customer intent: As an application developer, I want to learn how to access data in Azure Graph by using managed identities.
---

# Tutorial: Access Azure Graph from a secured app as the app

Learn how to access Azure Graph from a web app running on Azure App Service.

:::image type="content" alt-text="Diagram that shows accessing Azure Graph." source="./media/scenario-secure-app-access-microsoft-graph/web-app-access-graph.svg" border="false":::

You want to call Azure Graph for the web app. A safe way to give your web app access to data is to use a [system-assigned managed identity](../active-directory/managed-identities-azure-resources/overview.md). A managed identity from Azure Active Directory allows App Service to access resources through role-based access control (RBAC), without requiring app credentials. After assigning a managed identity to your web app, Azure takes care of the creation and distribution of a certificate. You don't have to worry about managing secrets or app credentials.

In this tutorial, you learn how to:

> [!div class="checklist"]
>
> * Create a system-assigned managed identity on a web app.
> * Add Azure Graph API permissions to a managed identity.
> * Call Azure Graph from a web app by using managed identities.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Prerequisites

* A web application running on Azure App Service that has the [App Service authentication/authorization module enabled](scenario-secure-app-authentication-app-service.md).

## Enable managed identity on app

If you create and publish your web app through Visual Studio, the managed identity was enabled on your app for you. In your app service, select **Identity** in the left pane and then select **System assigned**. Verify that **Status** is set to **On**. If not, select **Save** and then select **Yes** to enable the system-assigned managed identity. When the managed identity is enabled, the status is set to **On** and the object ID is available.

Take note of the **Object ID** value, which you'll need in the next step.

:::image type="content" alt-text="Screenshot that shows the system-assigned identity." source="./media/scenario-secure-app-access-microsoft-graph/create-system-assigned-identity.png":::

## Grant access to Azure Graph

When accessing the Azure Graph, the managed identity needs to have proper permissions for the operation it wants to perform. Currently, there's no option to assign such permissions through the Azure portal. The following script will add the requested Azure Graph API permissions to the managed identity service principal object.

# [PowerShell](#tab/azure-powershell)

```powershell
# Install the module. (You need admin on the machine.)
# Install-Module AzureAD.

# Your tenant ID (in the Azure portal, under Azure Active Directory > Overview).
$TenantID="<tenant-id>"
$resourceGroup = "securewebappresourcegroup"
$webAppName="SecureWebApp-20201102125811"

# Get the ID of the managed identity for the web app.
$spID = (Get-AzWebApp -ResourceGroupName $resourceGroup -Name $webAppName).identity.principalid

# Check the Azure Graph documentation for the permission you need for the operation.
$PermissionName = "User.Read.All"

Connect-AzureAD -AzureEnvironmentName AzureChinaCloud -TenantId $TenantID

# Get the service principal for Azure Graph.
$GraphServicePrincipal = Get-AzureADServicePrincipal -SearchString "Microsoft Graph"

# Assign permissions to the managed identity service principal.
$AppRole = $GraphServicePrincipal.AppRoles | `
Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}

New-AzureAdServiceAppRoleAssignment -ObjectId $spID -PrincipalId $spID `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole.Id
```

# [Azure CLI](#tab/azure-cli)

```azurecli
az login

webAppName="SecureWebApp-20201106120003"

spId=$(az resource list -n $webAppName --query [*].identity.principalId --out tsv)

graphResourceId=$(az ad sp list --display-name "Microsoft Graph" --query [0].objectId --out tsv)

appRoleId=$(az ad sp list --display-name "Microsoft Graph" --query "[0].appRoles[?value=='User.Read.All' && contains(allowedMemberTypes, 'Application')].id" --output tsv)

uri=https://graph.chinacloudapi.cn/v1.0/servicePrincipals/$spId/appRoleAssignments

body="{'principalId':'$spId','resourceId':'$graphResourceId','appRoleId':'$appRoleId'}"

az rest --method post --uri $uri --body $body --headers "Content-Type=application/json"
```

---

After executing the script, you can verify in the [Azure portal](https://portal.azure.cn) that the requested API permissions are assigned to the managed identity.

Go to **Azure Active Directory**, and then select **Enterprise applications**. This pane displays all the service principals in your tenant. In **All Applications**, select the service principal for the managed identity. 

If you're following this tutorial, there are two service principals with the same display name (SecureWebApp2020094113531, for example). The service principal that has a **Homepage URL** represents the web app in your tenant. The service principal without the **Homepage URL** represents the system-assigned managed identity for your web app. The **Object ID** value for the managed identity matches the object ID of the managed identity that you previously created.

Select the service principal for the managed identity.

:::image type="content" alt-text="Screenshot that shows the All applications option." source="./media/scenario-secure-app-access-microsoft-graph/enterprise-apps-all-applications.png":::

In **Overview**, select **Permissions**, and you'll see the added permissions for Azure Graph.

:::image type="content" alt-text="Screenshot that shows the Permissions pane." source="./media/scenario-secure-app-access-microsoft-graph/enterprise-apps-permissions.png":::

## Call Azure Graph (.NET)

The [DefaultAzureCredential](https://docs.azure.cn/dotnet/api/azure.identity.defaultazurecredential) class is used to get a token credential for your code to authorize requests to Azure Graph. Create an instance of the [DefaultAzureCredential](https://docs.azure.cn/dotnet/api/azure.identity.defaultazurecredential) class, which uses the managed identity to fetch tokens and attach them to the service client. The following code example gets the authenticated token credential and uses it to create a service client object, which gets the users in the group.

To see this code as part of a sample application, see the [sample on GitHub](https://github.com/Azure-Samples/ms-identity-easyauth-dotnet-storage-graphapi/tree/main/3-WebApp-graphapi-managed-identity).

### Install the Microsoft.Graph client library package

Install the [Microsoft.Graph NuGet package](https://www.nuget.org/packages/Microsoft.Graph) in your project by using the .NET Core command-line interface or the Package Manager Console in Visual Studio.

# [Command line](#tab/command-line)

Open a command line, and switch to the directory that contains your project file.

Run the install commands.

```dotnetcli
dotnet add package Microsoft.Graph
```

# [Package Manager](#tab/package-manager)

Open the project/solution in Visual Studio, and open the console by using the **Tools** > **NuGet Package Manager** > **Package Manager Console** command.

Run the install commands.
```powershell
Install-Package Microsoft.Graph
```

---

### Example

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Azure.Identity;​
using Microsoft.Graph.Core;​​
using System.Net.Http.Headers;

...

public IList<MSGraphUser> Users { get; set; }

public async Task OnGetAsync()
{
    // Create the Azure Graph service client with a DefaultAzureCredential class, which gets an access token by using the available Managed Identity.
    var credential = new DefaultAzureCredential();
    var token = credential.GetToken(
        new Azure.Core.TokenRequestContext(
            new[] { "https://graph.chinacloudapi.cn/.default" }));

    var accessToken = token.Token;
    var graphServiceClient = new GraphServiceClient(
        new DelegateAuthenticationProvider((requestMessage) =>
        {
            requestMessage
            .Headers
            .Authorization = new AuthenticationHeaderValue("bearer", accessToken);

            return Task.CompletedTask;
        }));

    List<MSGraphUser> msGraphUsers = new List<MSGraphUser>();
    try
    {
        var users =await graphServiceClient.Users.Request().GetAsync();
        foreach(var u in users)
        {
            MSGraphUser user = new MSGraphUser();
            user.userPrincipalName = u.UserPrincipalName;
            user.displayName = u.DisplayName;
            user.mail = u.Mail;
            user.jobTitle = u.JobTitle;

            msGraphUsers.Add(user);
        }
    }
    catch(Exception ex)
    {
        string msg = ex.Message;
    }

    Users = msGraphUsers;
}
```

## Clean up resources

If you're finished with this tutorial and no longer need the web app or associated resources, [clean up the resources you created](scenario-secure-app-clean-up-resources.md).

## Next steps

In this tutorial, you learned how to:

> [!div class="checklist"]
>
> * Create a system-assigned managed identity on a web app.
> * Add Azure Graph API permissions to a managed identity.
> * Call Azure Graph from a web app by using managed identities.

Learn how to connect a [.NET Core app](tutorial-dotnetcore-sqldb-app.md), [Python app](tutorial-python-postgresql-app.md), [Java app](tutorial-java-spring-cosmosdb.md), or [Node.js app](tutorial-nodejs-mongodb-app.md) to a database.


<!-- Update_Description: new article about scenario secure app access microsoft graph as app -->
<!--NEW.date: 12/21/2020-->