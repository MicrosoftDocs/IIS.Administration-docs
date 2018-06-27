---
uid: api/scope
---

# Scoping Configuration Changes/Access

The IIS configuration system is a hierarchy that extends from the root web server configuration to as far down as individual folders with a web application. The root configuration file is named _applicationHost.config_ and it resides in the IIS installation directory. The rest of a web application's configuration exists in web.config files within the application's directories. One benefit of using web.config files is that application's can introduce configuration at different paths without affecting the web server as a whole. At the same time, it is possible to define configuration in the root _applicationHost.config_ that targets only a specific path inside a specific web application. This means there are two different ways to think of configuration scoping.

1. Reading/modifying configuration for an arbitrary path of a web application.
2. Modifying configuration for a specific path, but placing the configuration in a configuration file at a higher level in the configuration hierarchy.
   * Example: Modifying a setting for *Default Web Site/MyApp/MyFolder* and placing it in the web.config file located in the root of *Default Web Site*

## Targeting a Specific Path Within a Web Application

Querying configuration at a certain path is done by using the *scope* query filter.

Example (query string is not URL encoded for readability):

```
GET https://localhost:55539/api/webserver/http-request-filtering?scope=Default Web Site/MyApp/MyFolder
```

This request will tell the API to query the request filtering settings for the MyFolder directory within the MyApp application of the Default Web Site. The request filtering resource that is returned will have the *id* necessary to modify the settings at that level within the configuration system.

## Setting Configuration and Storing It At A Higher Level

The most common use case of this capability is to set configuration settings that only web server administrators have access to touch. This is usually done for things that might affect the overall security of the Web Server. Usually these settings cannot be defined in the web.config files for an application so a web adminsitrator can configure the applications and tell the settings to reside in the _applicationHost.config_ file. This is implemented in the applicationHost.config file through the use of a \<location\> element that points to the configuration path that the settings should affect.

To achieve this, use *config_scope* in the body of a PATCH request being used to modify settings. The value of the *config_scope* property should be the web server path to the configuration file that the settings should reside in. For example the empty path "" refers to the root applicationHost.config file and the path "Default Web Site/MyApp" refers to the web.config file in the MyApp application within the Default Web Site.

Example:

Assume that the goal is to enable directory browsing for the path *Default Web Site/MyApp/MyFolder*, the id for this resource is *WXYZ*, and the settings should be stored in the applicationHost.config file.

```
PATCH https://localhost:55539/api/webserver/directory-browsing/WXYZ

{
    "enabled": "true",
    "config_scope": ""
}
```
