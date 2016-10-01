---
uid: security/access-tokens
---

# Access Tokens

Access tokens are randomly generated, base 64 encoded tokens that are required to access the API. If a request is made to the API without an access token, the API will respond with an HTTP 403 status code and will set the 'WWW-Authenticate' HTTP header to 'Bearer'. The API expects the access token to be in the 'Access-Token' header of every request. The value of the 'Access-Token' header should be 'Bearer ' followed by the access token being used to access the API.

```
    GET /api
    Access-Token: Bearer {Access-Token}
```

## Generating Tokens

Access tokens can be generated in multiple ways. The predominant method is through the built in [API Explorer](../api-explorer/index.md) for the API. The GUI has an "ACCESS KEYS" link at the top right. Clicking this link will lead to the access token management page. Here access tokens can be generated, deleted, and refreshed. Access tokens should be created with a descriptive purpose and one access token should be created for one user.

Alternatively, the API offers a method to generate API keys using the /security/api-keys route. 

## Securing Tokens

By default only members of the 'Administrators' and 'IIS Administrators' can generate access tokens. This requirement is set through the integrated security that the IIS Administration API utilizes. [Integrated Security](integrated/toc.md) provides more information on the integrated security used by the API. 