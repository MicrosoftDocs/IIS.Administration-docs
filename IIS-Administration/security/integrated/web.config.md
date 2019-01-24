---
title: Web.Config
description: This article is deprecated as of IIS Administration 2.0.0.
ms.date: 10/30/2016
uid: security/integrated/web.config
---

# Web.Config

This article is **deprecated** as of IIS Administration 2.0.0. and has been replaced by the [appsettings security section](../../configuration/appsettings.json.md)

The Microsoft IIS Administration API has access to all of the integrated security mechanisms offered by IIS. These settings are located in the web.config file that comes with the installation of the API. In this file [windows authentication](windows.md) and [authorization](authorization.md) requirements are specified. This file can be manipulated to customize who is allowed to access the API, for example, [windows authentication](windows.md) can be replaced with client certificate authentication. Any changes to the web.config file will require restarting the "Microsoft IIS Administration" service to take effect.

The web.config file is located at: 
`IIS Administration\<version>\Microsoft.IIS.Administration\web.config`

## Default Settings

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" arguments=".\Microsoft.IIS.Administration.dll" forwardWindowsAuthToken="true" stdoutLogEnabled="false" stdoutLogFile=".\logs\stdout" />
    <security>
      <authentication>
        <windowsAuthentication enabled="true" />
      </authentication>
      <authorization>
        <clear />
        <add accessType="Allow" roles="Administrators,IIS Administrators" />
      </authorization>
    </security>
  </system.webServer>
  <!-- 
       ALWAYS PROTECTED SECURITY AREA 
       THE HOST MUST PROVIDE ATHENTICATION
       
       [Windows Authentication]
       [Client Certificate Authentication]
  -->
  <location path="security">
    <system.webServer>
      <security>
        <authentication>
          <anonymousAuthentication enabled="false" />
          <windowsAuthentication enabled="true" />
        </authentication>
        <authorization>
          <clear />
          <add accessType="Deny" users="?" />
          <add accessType="Allow" roles="Administrators,IIS Administrators" />
        </authorization>
      </security>
    </system.webServer>
  </location>
  <!-- 
      API area 
      Protected by ACCESS TOKEN
      The host can provide additional authentication on top
  -->
  <location path="api">
    <system.webServer>
      <security>
        <authentication>
          <!-- Need for CORs -->
          <anonymousAuthentication enabled="true" />
        </authentication>
        <authorization>
          <!-- Need for CORs -->
          <add accessType="Allow" verbs="OPTIONS" users="*" />
        </authorization>
      </security>
    </system.webServer>
  </location>
</configuration>
```