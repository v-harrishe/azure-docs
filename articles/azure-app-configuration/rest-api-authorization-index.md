---
title: Azure App Configuration REST API - Authorization
description: Reference pages for authorization using the Azure App Configuration REST API


ms.service: azure-app-configuration
ms.topic: reference
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
---

# Authorization

Authorization refers to the procedure used to determine the permissions that a caller has when making a request. There are multiple authorization models. The authorization model that is used for a request depends on the [authentication](./rest-api-authentication-index.md) method that is used. The authorization models are listed below.

## HMAC

The [authorization model](./rest-api-authorization-hmac.md) model associated with HMAC authentication splits permissions into read-only or read-write. See the [HMAC authorization](./rest-api-authorization-hmac.md) page for details.

## Azure Active Directory

The [authorization model](./rest-api-authorization-azure-ad.md) associated with Azure Active Directory (Azure AD) authentication uses Azure RBAC to control permissions. See the [Azure AD authorization](./rest-api-authorization-azure-ad.md) page for details.



<!-- Update_Description: new article about rest api authorization index -->
<!--NEW.date: 12/21/2020-->