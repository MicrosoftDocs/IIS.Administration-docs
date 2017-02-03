---
uid: api/hal
---

# Hypertext Application Language (HAL)

The IIS Administration API includes special data in all of its resources called [Hypertext Application Language](http://stateless.co/hal_specification.html) (HAL). HAL is a set of conventions that provide a standardized way to link resources. HAL is only included in resources when the client includes _application/hal+json_ in the _Accept_ header of their HTTP requests.

## _links

The \_links property is the most prevalent form of HAL used in the API. The *_links* object contains members indicating how to retrieve related resources.

A resource including *_links*, some *_links* members have been excluded.
```
{
    "name": "Default Web Site",
    "id": "{id}",
    "physical_path": "%SystemDrive%\\inetpub\\wwwroot"
    .
    .
    .
    "application_pool": {
        "name": "DefaultAppPool",
        "id": "{app_pool_id}",
        "status": "started",
        "_links": {
            "self": {
                "href": "/api/webserver/application-pools/{app_pool_id}"
            }
        }
    },
    "_links": {
        "authentication": {
            "href": "/api/webserver/authentication/{authentication_id}"
        },
        "self": {
            "href": "/api/webserver/websites/{id}"
        },
        "webapps": {
            "href": "/api/webserver/webapps?website.id={id}"
        }
    }
}
```

This example resource is a web site in IIS. The *_links* property tells the consumer how to get to the authentication settings, how to view the web site's applications for the web site, and also the URI of the resource. 

### Self

Every resource that includes *_links* has a *self* link. This link provides the URI that the resource lives at. This URI is the same one that PATCH and DELETE requests should be sent to when updating or deleting the resource.

## Requesting HAL

HAL is an augmentation to the JSON data format. When a resource includes HAL it has a content type of _application/hal+json_. In order **to receive HAL** from the API, clients must specify _application/hal+json_ in the _Accept_ header of their HTTP requests.