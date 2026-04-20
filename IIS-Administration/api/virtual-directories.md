---
title: The Virtual Directory Resource
description: Virtual directories provide a way to create the virtual file hierarchy that a web site requires.
ms.date: 10/30/2016
uid: api/virtual-directories
---

# The Virtual Directory Resource

Virtual directories provide a way to create the virtual file hierarchy that a web site requires. The virtual directory API allows consumers to create, read, delete, or update their virtual directories.

**GET** _/api/webserver/virtual-directories/{id}_
```
{
    "location": "Default Web Site/demo-vdir",
    "path": "/demo-vdir",
    "id": "{id}",
    "physical_path": "c:\\inetpub\\wwwroot\\demo-vdir",
    "identity": {
        "username": "",
        "logon_method": "network_cleartext"
    },
    "webapp": {
        "location": "Default Web Site/",
        "path": "/",
        "id": "{webapp_id}"
    },
    "website": {
        "name": "Default Web Site",
        "id": "{website_id}",
        "status": "started"
    }
}
```

### Retrieving virtual directories for a Web Site

The virtual directories that belong to a web site can retrieved by providing the website id in the GET request. [Web site](sites.md) resources contain the link required to access their virtual directories in their [HAL](hal.md).

Listing the virtual directories of a website. **GET** */api/webserver/virtual-directories?website.id={website_id}*
```
{
    "virtual_directories": [
        {
            "location": "Default Web Site/",
            "path": "/",
            "id": "{id}",
            "_links": {
                "self": {
                    "href": "/api/webserver/virtual-directories/{id}"
                }
            }
        },
        {
            "location": "Default Web Site/demo-vdir",
            "path": "/demo-vdir",
            "id": "{id_1}",
            "_links": {
                "self": {
                    "href": "/api/webserver/virtual-directories/{id_1}"
                }
            }
        }
    ]
}
```

### Retrieving virtual directories for an application

The virtual directories that belong to an application can retrieved by providing the application id in the GET request. [Web application](applications.md) resources contain the link required to access their virtual directories in their [HAL](hal.md).

Listing the virtual directories of an application. **GET** */api/webserver/virtual-directories?webapp.id={webapp_id}*
```
{
    "virtual_directories": [
        {
            "location": "Default Web Site/demo-app/",
            "path": "/",
            "id": "{id}",
            "_links": {
                "self": {
                    "href": "/api/webserver/virtual-directories/{id}"
                }
            }
        },
        {
            "location": "Default Web Site/demo-app/app-level-vdir",
            "path": "/app-level-vdir",
            "id": "{id_1}",
            "_links": {
                "self": {
                    "href": "/api/webserver/virtual-directories/{id_1}"
                }
            }
        }
    ]
}
```

## Creating a Virtual Directory

Creating a virtual directory requires
* A web site or web application for the virtual directory to belong to
* A virtual path that is relative to the root of the web site or web app
* A physical path that specifies the directory on the file system which the virtual directory will reside in

Creating a virtual directory **POST** _/api/webserver/virtual-directories_
```
{
    "path": "demo-vdir",
    "physical_path": "C:\\inetpub\\wwwroot\\demo-vdir",
    "website": {
        "id": {website_id}
    }
}
```
