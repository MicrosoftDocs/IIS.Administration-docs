---
uid: security/integrated/web.config
---

# Web.Config

The Microsoft IIS Administration API has access to all of the integrated security mechanisms offered by IIS. These settings are located in the web.config file that comes with the installation of the API. In this file [windows authentication](windows.md) and [authorization](authorization.md) requirements are specified. This file can be manipulated to customize who is allowed to access the API, for example, [windows authentication](windows.md) can be removed entirely. Any changes to the web.config file will require restarting the "Microsoft IIS Administration" service to take effect.

The web.config file is located at: 
`IIS Administration\<version>\Microsoft.IIS.Administration\web.config`