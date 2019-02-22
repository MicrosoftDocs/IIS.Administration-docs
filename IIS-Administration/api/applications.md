---
title: The Web Application Resource
description: The application pool API allows consumers to create, read, delete, or update their application pools.
ms.date: 10/30/2016
uid: api/applications
---

# The Web Application Resource

Applications provide a method to differentiate sections of a web site. An application belongs to a single web site and will handle requests for the web site at the application path. The application pool API allows consumers to create, read, delete, or update their application pools.

**GET** _/api/webserver/webapps/{id}_
```
{
    "location": "Default Web Site/demo-app",
    "path": "/demo-app",
    "id": "{id}",
    "physical_path": "c:\\inetpub\\wwwroot\\demo-app",
    "enabled_protocols": "http",
    "website": {
        "name": "Default Web Site",
        "id": "{website_id}",
        "status": "started"
    },
    "application_pool": {
        "name": "DefaultAppPool",
        "id": "{application_pool_id}",
        "status": "started"
    },
    "_links": {
        "authentication": {
            "href": "/api/webserver/authentication/{authentication_id}"
        },
        "authorization": {
            "href": "/api/webserver/authorization/{authorization_id}"
        },
        "default_document": {
            "href": "/api/webserver/default-documents/{default_document_id}"
        },
        "directory_browsing": {
            "href": "/api/webserver/directory-browsing/{directory_browsing_id}"
        },
        "handlers": {
            "href": "/api/webserver/http-handlers/{handlers_id}"
        },
        "ip_restrictions": {
            "href": "/api/webserver/ip-restrictions/{ip_restrictions_id}"
        },
        "modules": {
            "href": "/api/webserver/http-modules/{modules_id}"
        },
        "request_filtering": {
            "href": "/api/webserver/http-request-filtering/{request_filtering_id}"
        },
        "request_tracing": {
            "href": "/api/webserver/http-request-tracing/{request_tracing_id}"
        },
        "response_compression": {
            "href": "/api/webserver/http-response-compression/{response_compression_id}"
        },
        "response_headers": {
            "href": "/api/webserver/http-response-headers/{response_headers_id}"
        },
        "ssl": {
            "href": "/api/webserver/ssl-settings/{ssl_id}"
        },
        "static_content": {
            "href": "/api/webserver/static-content/{static_content_id}"
        },
        "vdirs": {
            "href": "/api/webserver/virtual-directories?webapp.id={id}"
        }
    }
}
```

### Retrieving applications for a web site

The applications that belong to a web site can retrieved by providing the website id in the GET request. [Web site](sites.md) resources contain the link required to access their applications in their [HAL](hal.md).

Listing the applications for a website. **GET** */api/webserver/webapps?website.id={website_id}*
```
{
    "webapps": [
        {
            "location": "Default Web Site/",
            "path": "/",
            "id": "{id}",
            "_links": {
                "self": {
                    "href": "/api/webserver/webapps/{id}"
                }
            }
        },
        {
            "location": "Default Web Site/demo-app",
            "path": "/demo-app",
            "id": "{id_1}",
            "_links": {
                "self": {
                    "href": "/api/webserver/webapps/{id_1}"
                }
            }
        }
    ]
}
```

## Creating an Application

Creating an application requires:
* website: The web site that the application should be created for.
* path: The virtual path that the application should exist at relative to the root of the web site.
* physical_path: The directory that the application should exist in in the file system. The directory must exist.

Creating an application. **POST** _/api/webserver/webapps_
```
{
    "path": "demo-app",
    "physical_path": "C:\\inetpub\\wwwroot\\demo-app",
    "website: {
        "id": {website_id}
    }
}
```

### Using a specific application pool

To create an application in a specific application pool, add the **application_pool** property to the request body.

Creating an application for a specific application pool. **POST** _/api/webserver/webapps_
```
{
    "path": "demo-app",
    "physical_path": "C:\\inetpub\\wwwroot\\demo-app",
    "website: {
        "id": {website_id}
    },
    "application_pool": {
        "id": {application_pool_id}
    }
}
```