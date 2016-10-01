---
uid: configuration/appsettings.json
---

# Application Settings (appsettings.json)

The appsettings.json file specifies application specific settings that alter the behavior of the Microsoft IIS Administration API. Any changes to the appsettings.json file require restarting the "Microsoft IIS Administration" service to take effect.

The appsettings.json file is located at: 
`IIS Administration\<version>\Microsoft.IIS.Administration\config\appsettings.json`

## CORS

The cors section specifies what origins are allowed access to the API. By default the API only allows cross origin requests from the web based management UI we are building at [manage.iis.net](https://manage.iis.net). If the wild card character **&ast;** is provided as the origin, that rule will apply to all origins.

The following enables CORS for [manage.iis.net](https://manage.iis.net)
```
  "cors": {
    "rules": [
      {
        "origin": "https://manage.iis.net",
        "allow": true
      }
    ]
  }
```