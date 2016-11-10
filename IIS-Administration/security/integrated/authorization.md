---
uid: security/integrated/authorization
---

# Authorization

By default the Microsoft IIS Administration API only allows access to windows users that are part of the _Administrators_ and _IIS Administrators_ groups. The _IIS Administrators_ group is generated during installation of the API and the installing user is automatically added to the group.

## Route Based Authorization

### /security

The /security route is security critical, therefore the [web.config](web.config.md) file specifies the security settings for this route separately. It is recommended not to lift the requirement for users to be in the _Administrators_ or _IIS Administrators_ group to access this route.

This route is used for generating access tokens.

### /api

On top of the default authorization requirements, the /api route requires an access-token with every request. Some users may wish to modify the windows based authorization requirements of the api route to add or remove roles. This is a viable option for those wishing to use the API to administor their servers.

The /api route allows anonymous access for HTTP _OPTIONS_ requests. This is necessary for CORS requests from allowed origins to be successful. Without this rule, browser based GUIs would not be capable of using the API.