---
uid: configuration/appsettings.json
---

# Application Settings (appsettings.json)

All of the application's settings are contained in a file named appsettings.json. Any changes to the appsettings.json file will require restarting the "Microsoft IIS Administration" service to take effect.

The appsettings.json file is located at: 
`IIS Administration\<version>\Microsoft.IIS.Administration\config\appsettings.json`

## CORS

[CORS](https://www.w3.org/TR/cors/) policies allow browser based applications to send requests to the Microsoft IIS Administration API.  By default, [manage.iis.net](https://manage.iis.net) is the only origin that is allowed in the API's CORS policy.

### Default Settings

The IIS Administration API will not allow CORS for any origin if there are no _cors_ settings present.

### Format

__rules__: A set of CORS rules to control how the API shares resources.

__origin__: The origin, as defined in the [CORS](https://www.w3.org/TR/cors/) specification, to allow or deny. If the wild card character, **&ast;**, is provided as the origin, that rule will apply to all origins.

__allow__: Indicates whether resources should be shared to the specified origin.

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

## Files

Multiple endpoints require interacting with the file system, such as creating a web site in an existing directory (read) or uploading the content of a file (write). These configuration settings provide a method to restrict these file system interactions. A set of file system locations that are visible to the API are specified. These paths can have read and or write priveleges associated with them. 

### Default Settings

The IIS Administration API will allow read access to %systemdrive%\inetpub there are no _files_ settings present.

### Format

__locations__: A set of file system locations and associated rights specifying what operations are allowed to be performed through the API.

__alias__: A name for the location.

__path__: A root path to assign the list of claims. All files or directories under this path inherit the list of claims unless overridden with a more specific path.

__claims__: Specifies what operations are allowed to be performed on files directories under the path. An empty set of claims means no access will be allowed to that location.

The following settings allow read/write access to _%systemdrive%\inetpub_ and read access to _%systemdrive%\shared_images_

```
  "files": {
    "locations": [
      {
        "alias": "inetpub",
        "path": "%systemdrive%\\inetpub",
        "claims": [
          "read",
          "write"
        ]
      },
      {
        "alias": "read-only",
        "path": "%systemdrive%\\shared_images",
        "claims": [
          "read"
        ]
      }
    ]
  }
```

## Complete Example

```
{
  "host_id": "",

  "host_name": "My instance of the IIS Administration API",

  "administrators": [
    "Administrators",
    "IIS Administrators"
  ],

  "logging": {
    "enabled": true,
    "file_name": "log-{Date}.txt",
    "min_level": "Error",
    "path": null
  },

  "auditing": {
    "enabled": true,
    "file_name": "audit-{Date}.txt",
    "path": null
  },

  "cors": {
    "rules": [
      {
        "origin": "https://manage.iis.net",
        "allow": true
      },
      {
        "origin": "https://My-Organizations-Custom-UI.com",
        "allow": true
      }
    ]
  },

  "files": {
    "locations": [
      {
        "alias": "inetpub",
        "path": "%systemdrive%\\inetpub",
        "claims": [
          "read",
          "write"
        ]
      }
    ]
  }
}
```