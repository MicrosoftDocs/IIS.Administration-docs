---
title: 
description: 
ms.date: 10/30/2016
uid: api/api-keys
---

# Api Keys

The Microsoft IIS Administration API allows generation and viewing of API key information through secured endpoints.

## Access Token Info (/api/access-token)

This endpoint provides non-sensitive information about the access token being used for the request.

GET _/api/access-token_
```
{
    "id": "{access-token-id}",
    "expires_on": "2018-02-02T16:25:31.4003337Z",
    "type": "SWT",
    "api_key": {
        "purpose": "Admin",
        "id": "{api-key-id}",
        "_links": {
            "self": {
                "href": "/security/api-keys/{api-key-id}"
            }
        }
    }
}
```

## API Keys (/security/api-keys)

This endpoint allows for programmatic creation and deletion of API keys. This is useful for generating API keys that will only exist for the scope of a scripting session or for managing API keys with a central application. The API keys endpoint is under the [_/security_ location](../security/integrated/authorization.md#route-based-authorization), which means by default only _Administrators_ and _IIS Administrators_ have access to it.

GET _/security/api-keys/{api-key-id}_
```
{
    "purpose": "Admin",
    "id": "{api-key-id}",
    "created_on": "2017-02-02T16:25:31.4003337Z",
    "last_modified": "2017-02-02T16:25:31.4003337Z",
    "expires_on": "2018-02-02T16:25:31.4003337Z",
    "_links": {
        "access_token": {
            "href": "/security/access-tokens/{access-token-id}"
        }
    }
}
```

## Creating an API Key

Creating an API key is a special task that requires two requests. The extra request is used to prevent [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)). First the user must query the API keys endpoint and recieve a special token from the _XSRF-TOKEN_ header. Then the user can create the API key by specifying the _XSRF-TOKEN_ header in the creation request.

**Note:** XSRF-TOKEN is sent as an HTTP header.

```
Client                                                                                      Server
|                                                                                                |
| GET /security/api-keys                                                                         |
| ---------------------------------------------->                                                |
|                                                                                                |
|                                                                                                |
|                                                            XSRF-TOKEN: {value}                 |
|                                                <---------------------------------------------- |
|                                                                                                |
|                                                                                                |
| POST /security/api-keys                                                                        |
|      XSRF-TOKEN: {value}                                                                       |
|                                                                                                |
|      {                                                                                         |
|          "purpose": "Admin"                                                                    |
|      }                                                                                         |
| ---------------------------------------------->                                                |
|                                                                                                |
|                                                                                                |
|                                                   {                                            |
|                                                     "access_token" : "{access-token-value}"    |
|                                                   }                                            |
|                                                <---------------------------------------------- |
```

### POST Request for API Key Creation

The POST request section of API key generation is where parameters for the API key can be specified. This is an example of a POST body that creates an API key with a specific purpose and expiration date.

```
{
    "purpose": "Admin",
    "expires_on": "2018-02-02T16:25:31.4003337Z",
}
```

## Deleting an API Key

API Keys can be deleted by performing a DELETE request at the api-key's endpoint _/security/api-keys/{api-key-id}_