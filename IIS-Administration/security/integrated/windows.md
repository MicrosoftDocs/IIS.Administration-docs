---
uid: security/integrated/windows
---

# Windows Authentication

Out of the box, the Microsoft IIS Administration API utilizes IIS's windows authentication module to authenticate users. These settings are specified in the [web.config](web.config.md) file. The windows authentication module enables the API to be restricted to users in the _Administrators_ and _IIS Administrators_ groups. By manipulating the [web.config](web.config.md) file you can [remove the requirements](remove-integrated.md) for windows auth. When windows auth is removed, the API can be accessed by anonymous users and access tokens can be used as the sole mechanism for security.

## Browser Access

Depending on browser settings, users who access the API via the built-in [API Explorer](../../api explorer/toc.md) may notice a login prompt. This is the browser's mechanism authentication with windows credentials. Domain users should include their domain name when specifying their username, for example "contoso\johndoe". 