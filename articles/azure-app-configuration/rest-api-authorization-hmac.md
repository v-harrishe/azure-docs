---
title: Azure App Configuration REST API - HMAC authorization
description: Use HMAC for authorization against Azure App Configuration using the REST API


ms.service: azure-app-configuration
ms.topic: reference
author: rockboyfor
ms.date: 12/21/2020
ms.testscope: yes|no
ms.testdate: 12/21/2020null
ms.author: v-yeche
---

# HMAC authorization - REST API reference

When HMAC authentication is used, operations fall in to one of two categories, read or write. Read-write access keys grant permission to call all operations. Read-only access keys grant permission to call only read operations. Whether an access key is read-only or read-write is determined by its `readOnly` property. Any attempt to make a write request with a read-only access key will result in the request being unauthorized.

## Obtaining access keys

The specification describing access keys and the API used to obtain them is detailed in the Azure App Configuration resource provider spec [here](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/appconfiguration/resource-manager/Microsoft.AppConfiguration/stable/2019-10-01/appconfiguration.json). Access keys are obtained via the "ConfigurationStores_ListKeys" operation.

## Errors

```http
HTTP/1.1 403 Forbidden
```

**Reason:** The access key used to authenticate the request does not provide the required permissions to perform the requested operation.
**Solution:** Obtain an access key that provides permission to perform the requested operation and use it to authenticate the request.



<!-- Update_Description: new article about rest api authorization hmac -->
<!--NEW.date: 12/21/2020-->