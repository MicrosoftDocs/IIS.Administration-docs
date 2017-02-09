---
uid: api/crud
---

# CRUD (Create, Read, Update, Delete)

The IIS Administration API provides direct access to resources on the system. Many of these resources allow create, read, update and delete operations. The REST API maps CRUD operations to HTTP methods. The following table specifies which HTTP method maps to which operation. 


| CRUD Operation | HTTP Method |
|--------|---------------------|
| Create | POST                |
| Read   | GET                 |
| Update | PATCH / PUT         |
| Delete | DELETE              |

## Create (POST)

Resources are created by sending HTTP POST requests to the API. The type of resource is determined by the URL of the request. The body of the request should contain a JSON object describing the resource to create. The object in the request body determines the initial state of the resource will be when it is created. Some resources require certain properties be provided when they are created, others can be created with an empty JSON object.

Creating a resource while setting the name property. **POST**
```
{
    "name": "Example Resource Name"
}
```

### Creating a resource that belongs to another

Sometimes resources are created that are meant to belong to another resource. For example, if _applications_ must belong to a web site and someone wanted to create a new _application_ then they would specify that relationship during the creation of the application as shown below.

Creating an application for a web site. **POST**
```
{
    "path": "/MyApp",
    "physical_path": "c:/sites/mysite/myapp",
    **"website"**: {
        "id": {website_id}
    }
}
```

## Read (GET)

Resources are retrieved by performing HTTP GET requests. There are two main methods to retrieve resources. The first method involves requesting a list of resources, the second method is when a single resource is requested. Requests to a single resource are marked by the presence of the resource **id** in the URL of the request. Sometimes, singular resources can also be specified through query string paremeters in the URL. This behavior depends on the individual API endpoint.

### Retrieving multiple resources

Reading lists of resources is done by requesting a resource endpoint without specifying an individual resources **id**. Sometimes resources require query string parameters or else they cannot produce valid lists. For example IIS applications live at the _/api/webserver/webapps_ endpoint, but requesting that endpoint alone would produce no information. This is because a web site must be specified to tell the API which applications should be shown. So consumers would request */api/webserver/webapps?website.id={website_id}* to see a list of applications.

Retrieving a list of resources. **GET** _/api/websites_
```
{
    "websites": [
        {
            "name": "Default Web Site",
            "id": "{id}",
            "status": "started",
            "_links": {
                "self": {
                    "href": "/api/webserver/websites/{id}"
                }
            }
        },
        {
            "name": "My Site",
            "id": "{id_1}",
            "status": "started",
            "_links": {
                "self": {
                    "href": "/api/webserver/websites/{id_1}"
                }
            }
        }
        {
            "name": "docs",
            "id": "{id_2}",
            "status": "started",
            "_links": {
                "self": {
                    "href": "/api/webserver/websites/{id_2}"
                }
            }
        }
    ]
}
```

### Retrieving individual resources

Resources are retrieved on an individual basis by providing the **id** of the resource in the URL of the resource endpoint. Some API endpoints also allow specifying individual resources by providing uniquely identifying query string parameters. For example, a file can be retrieved by providing the **id** of the file in the URL or by providing the **physical_path** of the file.

The file resource allows multiple methods to retrieve individual files:
* */api/files/{id}*
* */api/files?physical_path={physical path of the file}*

The files endpoint provides this behavior because only one file can exist for any given physical path, so it is a **uniquely identifying** query string parameter.

## Update (PATCH / PUT)

Updates are performed by issuing HTTP PATCH requests to the URL that the resource is located at. When a PATCH request is performed, the properties of the request body are read, and if the resource has a property with the same name the property of the resource will be set to the new value.

### Example resource before PATCH
```
{
    "name": "My Site",
    "id": "12345",
    "physical_path": "c:\\sites\\mysite"
    "_links": {
        "self": {
            "href": "/api/webserver/websites/{12345}"
        }
    }
}
```

### Performing the PATCH request

Patch request to update the name of the resource. **PATCH** _/api/webserver/websites/12345_
```
{
    "name": "My Site 2"
}
```

### Resource after PATCH
```
{
    "name": "My Site 2",
    "id": "12345",
    "physical_path": "c:\\sites\\mysite"
    "_links": {
        "self": {
            "href": "/api/webserver/websites/{12345}"
        }
    }
}
```

## Delete (DELETE)

Resources are deleted by sending an HTTP DELETE request to the URL that the resource is located at. This is the URL that contains the **id** of the resource.