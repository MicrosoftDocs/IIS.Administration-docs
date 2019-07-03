---
uid: security/access-tokens
ms.date: 03/07/2017
---

# Access Tokens

Every request to the API requires an access token. If a request is made to the API without an access token, the API will respond with an HTTP 403 status code and will set the 'WWW-Authenticate' HTTP header to 'Bearer'. The API expects the access token to be in the _Access-Token_ header of every request. The value of the _Access-Token_ header should be 'Bearer ' followed by the access token being used to access the API.

```
    GET /api
    Access-Token: Bearer {Access-Token}
```

## Generating Tokens

Access tokens can be generated in multiple ways. The predominant method is through the built in [API Explorer](../api-explorer/index.md). Access tokens can also be generated programatically through the [API keys](../api/api-keys.md) endpoint. 

### Generating Tokens Through The API Explorer

The API explorer has an "ACCESS KEYS" link at the top right. Clicking this link will lead to the access token management page. Here access tokens can be generated, deleted, and refreshed. Access tokens should be created with a descriptive purpose and one access token should be created for one user.

![Generating an access token][generate]

Alternatively, the API offers a method to programatically [generate API keys](../api/api-keys.md).

## Securing Tokens

By default only members of the 'Administrators' and 'IIS Administrators' can generate access tokens. This requirement is set through the integrated security that the IIS Administration API utilizes. [Windows Authentication](integrated/windows.md) provides more information on the integrated security used by the API. 




[generate]: _static/generate-access-token.png "Generating an access token"