---
uid: api/installing-features
ms.date: 05/02/2017
---

# Installing IIS Components

IIS and its components are exposed by Windows as optional features. This provides a way for users to enable only the features of IIS that they need for their sites to operate. As a side effect, when configuring IIS through the API it is possible that a feature, for example Windows Authentication, may not yet be installed on the machine. Historically, enabling IIS features was done through DISM.exe, PowerShell commands, or the Windows Optional Feature UI. The IIS Administration API exposes a simple method to install/uninstall these features so that consumers do not have to change environments in order to enable the features that they depend on.

## Checking If a Feature Is Installed

The first step of managing a feature is to check if it is installed. For any IIS feature we can determine this by sending a request to its API endpoint. If the endpoint returns a _200 OK_ response, the feature is installed. If the feature is not installed, the API will return a _404 Not Found_ response with a _Feature Not Installed_ JSON error object in the body. This example will use the default document feature of IIS. We check to see if the default document is enabled by sending a GET request to the default document endpoint. We use _scope_ in the query string with an empty value to specify that we are targetting the web server scope.

**GET** _/api/webserver/default-documents?scope=_

**Feature Is Not Installed**

```
404 Not Found


{
    "title": "Not found",
    "detail": "IIS feature not installed",
    "name": "Default Document",
    "status": "404"
}
```

**Feature Is Installed**

```
200 OK


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

Feature installation is performed by issuing a _POST_ request to the feature's endpoint. Some IIS features such as the [Central Certificate Store](centralized-certificates.md) require initial settings to be provided when installing. As an example, suppose the default document feature was returning the _404 Feature Not Installed_ response. Sending a _POST_ request to to the default documents endpoint installs the feature and then returns the features settings.

**POST** _/api/webserver/default-documents_

**HTTP Response**
```
201 CREATED
Location: /api/webserver/default-documents/{id}


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

## Uninstalling a Feature

IIS features can be uninstalled by issuing a _DELETE_ request to the feature at the web server level. For IIS features that support configuration at web site and application levels, one can verify that the object represents the web server scope by ensuring the _scope_ field is empty. 

First get the URI of the feature to uninstall.

**GET** _/api/webserver_

```
{
    "id": "{id}",
    "_links": {
        ... // Other features omitted
        "default_document": {
            "href": "/api/webserver/default-documents/{def-doc-id}"
        }
    }
}
```

Then, issue the _DELETE_ request to the features endpoint

**DELETE** _/api/webserver/default-documents/{def-doc-id}_

`
204 NO CONTENT
`