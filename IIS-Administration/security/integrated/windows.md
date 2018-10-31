---
title: Windows Authentication
description: This article is deprecated as of IIS Administration 2.0.0.
ms.date: 10/30/2016
uid: security/integrated/windows
---

# Windows Authentication

This article is **deprecated** as of IIS Administration 2.0.0. and has been replaced by the [appsettings security section](../../configuration/appsettings.json.md)

Out of the box, the Microsoft IIS Administration API utilizes IIS's windows authentication module to authenticate users. These settings are specified in the [web.config](web.config.md) file. The windows authentication module restricts access to the API to to users in the _Administrators_ and _IIS Administrators_ groups. By manipulating the [web.config](web.config.md) file, one can alter the requirements for windows authentication. If windows authentication is removed access tokens become the sole mechanism for security.

## Browser Access

Depending on browser settings, users who access the API via the built-in [API Explorer](../../api-explorer/index.md) may notice a login prompt. This is the browser's mechanism for authenticating with windows credentials. Domain users should include their domain name when specifying their username, for example *contoso\johndoe*. 