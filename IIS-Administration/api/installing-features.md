---
uid: api/installing-features
---

# Installing IIS Components

IIS and its components are exposed by Windows as optional features. This provides a way for users to enable only the features of IIS that they need for their sites to operate. As a side effect, when configuring IIS through the API it is possible that a feature, for example Windows Authentication, may not yet be installed on the machine. Historically, enabling IIS features was done through DISM.exe, PowerShell commands, or the Windows Optional Feature UI. The IIS Administration API exposes a simple method to install/uninstall these features so that consumers do not have to change environments in order to enable the features that they depend on.

## Checking If a Feature Is Installed

The first step of managing a feature is to check if it is installed. For any IIS feature we can determine this by sending a request to its API endpoint. If the endpoint returns a _200 OK_ response, the feature is installed. If the feature is not installed, the API will return a _404 Not Found_ response with a _Feature Not Installed_ JSON error object in the body. This example will use the default document feature of IIS. We check to see if the default document is enabled by sending a GET request to the default document endpoint. We use _scope_ in the query string with an empty value to specify that we are targetting the web server scope.

### Feature Is Not Installed

**GET** _/api/webserver/default-documents?scope=_

_404 Not Found_

```
{
    "title": "Not found",
    "detail": "IIS feature not installed",
    "name": "Default Document",
    "status": "404"
}
```

### Feature Is Installed

**GET** _/api/webserver/default-documents?scope=_

_200 OK_ 

```
{
    "id": "{id}",
    "enabled": "true",
    "scope": "",
    "metadata": {
        "is_local": "true",
        "is_locked": "false",
        "override_mode": "allow",
        "override_mode_effective": "allow"
    },
    "website": null
}
```

## Installing a Feature

For most features, installation as simple as sending a POST request with an empty JSON object as the body. Some IIS features such as the [Central Certificate Store](centralized-certificates.md) require initial settings to be provided when installing. The default document feature is one of the IIS components that can be enabled through a basic POST request. Supposing the default document endpoint was returning the _404 Feature Not Installed_ response, we can send a POST request to install it.

**POST** _/api/webserver/default-documents_

**Request Body:**
```
{  }
```

**Response Body:**
```
{
    "id": "{id}",
    "enabled": "true",
    "scope": "",
    "metadata": {
        "is_local": "true",
        "is_locked": "false",
        "override_mode": "allow",
        "override_mode_effective": "allow"
    },
    "website": null
}
```