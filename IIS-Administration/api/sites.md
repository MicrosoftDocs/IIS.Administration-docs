---
uid: api/sites
---

# The Web Site Resource

Web sites are a core entity of IIS that determine where and how requests will be handled. The web site API allows consumers to create, read, delete, or update their web sites. This is suitable for scripting automated deployments or making alterations to existing resources.

**GET** _/api/webserver/websites/{id}_
```
{
    "name": "Default Web Site",
    "id": "{id}",
    "physical_path": "%SystemDrive%\\inetpub\\wwwroot",
    "key": "1",
    "status": "started",
    "server_auto_start": "true",
    "enabled_protocols": "http",
    "limits": {
        "connection_timeout": "120",
        "max_bandwidth": "4294967295",
        "max_connections": "4294967295",
        "max_url_segments": "32"
    },
    "bindings": [
        {
            "protocol": "https",
            "binding_information": "*:443:",
            "ip_address": "*",
            "port": "443",
            "hostname": "",
            "certificate": {
                "name": "Web Hosting Certificate",
                "id": "{certificate_id}",
                "issued_by": "CN=localhost",
                "subject": "CN=localhost",
                "thumbprint": "2F6C0E796B8DAC4A1DDBF59F1714A19D9520B88A",
                "valid_to": "2022-01-09T00:00:00Z"
            },
            "require_sni": "false"
        },
        {
            "protocol": "http",
            "binding_information": "*:80:",
            "ip_address": "*",
            "port": "80",
            "hostname": ""
        },
        {
            "protocol": "net.tcp",
            "binding_information": "808:*"
        }
    ],
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
        "delegation": {
            "href": "/api/webserver/feature-delegation?website.id={id}"
        },
        "directory_browsing": {
            "href": "/api/webserver/directory-browsing/{directory_browsing_id}"
        },
        "files": {
            "href": "/api/webserver/files/{files_id}"
        },
        "handlers": {
            "href": "/api/webserver/http-handlers/{handlers_id}"
        },
        "ip_restrictions": {
            "href": "/api/webserver/ip-restrictions/{ip_restrictions_id}"
        },
        "logging": {
            "href": "/api/webserver/logging/{logging_id}"
        },
        "modules": {
            "href": "/api/webserver/http-modules/{modules_id}"
        },
        "request_filtering": {
            "href": "/api/webserver/http-request-filtering/{request_filtering_id}"
        },
        "request_monitor": {
            "href": "/api/webserver/http-request-monitor/requests?website.id={request_monitor_id}"
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
            "href": "/api/webserver/virtual-directories?website.id={id}"
        },
        "webapps": {
            "href": "/api/webserver/webapps?website.id={id}"
        }
    }
}
```

## Web Site Bindings

The bindings of a web site determine what ports, protocols, and hostnames the site will listen for. At the least, a binding must specify a protocol, ip adddress, and a port. The *binding_information* property can be used to specify the ip address, port, and hostname. The format of *binding_information* is '{ip_address}:{port}:{hostname}' for HTTP and HTTPS protocols. A certificate is required for HTTPS bindings.

## Creating a Web Site

Creating a web site requires a physical path to store the web site, the name of the web site, and a set of bindings that the web site should listen on.

### Creating an HTTP Enabled Web Site

Creating a web site that listens on port 8081. **POST** _/api/webserver/websites_
```
{
    "name": "Demonstration Site",
    "physical_path": "C:\\inetpub\\wwwroot\\DemonstrationSite",
    "bindings": [
        {
            "protocol": "HTTP",
            "port": "8081",
            "ip_address": *
        }
    ]
}
```

### Creating an HTTPS Enabled Web Site

To create a site with an HTTPS binding, a certificate must provided in the binding object. Certificates are a resource available through the _/api/certificates_ endpoint. To specify the desired certificate in the binding, add the certificate resource and specify the certificate **id** as shown below.

Creating a web site that listens for HTTPS requests on port 8082.  **POST** _/api/webserver/websites_
```
{
    "name": "Demonstration Site",
    "physical_path": "C:\\inetpub\\wwwroot\\DemonstrationSite",
    "bindings": [
        {
            "protocol": "HTTPS",
            "port": "8082",
            "ip_address": *,
            "certificate": {
                "id": {certificate_id}
            }
        }
    ]
}
```

### Creating in a specific Application Pool

To specify which application pool that a web site should be created for, add the *application_pool* property to the body of the request. The application pool is identified solely by its *id* property.

Creating a web site for a specific application pool. **POST** _/api/webserver/websites_
```
{
    "name": "Demonstration Site",
    "physical_path": "C:\\inetpub\\wwwroot\\DemonstrationSite",
    "bindings": [
        {
            "protocol": "HTTP",
            "port": "8081",
            "ip_address": *
        }
    ],
    "application_pool": {
        "id": {application_pool_id}
    }
}
```

## Updating a Web Site

Updating web sites is done through patch requests. Sending a patch request with the web site in the desired state will update the web site on the server to match.

### Adding a binding

A common reason to update a web site is to add a binding. As an example, let us say that there is a web site with a single binding on port 80, and we want to add a binding that listens on port 8080. We should send a patch request that contains a _bindings_ list with both the bindings that we desire. Since the _bindings_ property is a list, the bindings that already exist on the web site should be sent as part of the request so that they are not lost.

Adding a binding to a web site. **PATCH** _/api/webserver/websites/{id}_
```
{
    "bindings": [
        {
            "protocol": "HTTP",
            "port": "80",
            "ip_address": *
        },
        {
            "protocol": "HTTP",
            "port": "8080",
            "ip_address": *
        }
    ]
}
```