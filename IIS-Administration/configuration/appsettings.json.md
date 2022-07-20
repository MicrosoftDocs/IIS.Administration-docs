---
uid: configuration/appsettings.json
ms.date: 07/03/2017
---

# Application Settings (appsettings.json)

All of the application's settings are contained in a file named appsettings.json. Any changes to the appsettings.json file will require restarting the "Microsoft IIS Administration" service to take effect.

The appsettings.json file is located at: 
`%SystemDrive%\Program Files\IIS Administration\<version>\Microsoft.IIS.Administration\config\appsettings.json`

## CORS

[CORS](https://www.w3.org/TR/cors/) policies allow browser based applications to send requests to the Microsoft IIS Administration API. 

### Default Settings

The IIS Administration API will not allow CORS for any origin if there are no _cors_ settings present.

### Format

For example, the following enables CORS:
```
  "cors": {
    "rules": [
      {
        "origin": "https://contoso.com",
        "allow": true
      }
    ]
  }
```

__rules__: A set of CORS rules to control how the API shares resources.

* **origin**: The origin, as defined in the [CORS](https://www.w3.org/TR/cors/) specification, to allow or deny. If the wild card character, \*, is provided as the origin, that rule will apply to all origins.

* __allow__: Indicates whether resources should be shared to the specified origin.

## Files

Multiple endpoints require interacting with the file system, such as creating a web site in an existing directory (read) or uploading the content of a file (write). These configuration settings provide a method to restrict these file system interactions. A set of file system locations that are visible to the API are specified. These paths can have read and or write priveleges associated with them. 

### Default Settings

The IIS Administration API will allow read access to %systemdrive%\inetpub if there are no _files_ settings present.

### Format

The following settings allow read/write access to _%systemdrive%\inetpub_

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
      }
    ]
  }
```

**skip_resolving_symbolic_links**: A flag specifying whether the system will resolve symbolic links when determining whether a path is allowed. By default this flag is _false_, meaning symbolic links **will** be resolved.

__locations__: A set of file system locations and associated rights specifying what operations are allowed to be performed through the API.

 * __alias__: A name for the location.

 * __path__: A root path to assign the list of claims. All files or directories under this path inherit the list of claims unless overridden with a more specific path.

 * __claims__: Specifies what operations are allowed to be performed on files directories under the path. An empty set of claims means no access will be allowed to that location.

## Security

The security section was introduced in IIS Administration 2.0.0. This section specifies the requirements to access the API.

### Default Settings

By default the API requires all requests to have valid Windows credentials as indicated by the _require_windows_authentication_ flag. Access to the API's resources, such as websites and applications, and access key manipulation require the user to be in the _administrators_ API role. High privilege operations require the user to be in the _owners_ role. When the API is installed, the _administrators_ and _owners_ roles are automatically populated with the user that executed the installer.

### Format

```
"security": {
    "require_windows_authentication": true,
    "users": {
      "administrators": [
      ],
      "owners": [
      ]
    },
    "access_policy": {
      "api": {
        "users": "administrators",
        "access_key": true
      },
      "api_keys": {
        "users": "administrators",
        "access_key": false
      },
      "system": {
        "users": "owners",
        "access_key": true
      }
    }
  }
```

**require_windows_authentication**: A boolean value that specifies whether valid Windows authentication is required for **all** requests to the API. If true, any request that is not Windows authenticated will be rejected. If false, Windows authentication requirements are determined by the _access_policy_ settings.

**users**: A mapping between Windows users/groups and roles within the API. Any role can be added, but by default the appsettings.json file contains _administrators_ and _owners_. These roles are used in the _access_policy_ section to govern access to different sections of the API.

**access_policy**: Access policies specify a set of requirements to access areas within the API. The IIS Administration API comes with three different access policies, _api_, _api_keys_, and _system_.

 * **api**: This access policy is for API resources such as web sites, application pools, and files.

 * **api_keys**:  This access policy is for manipulating API keys. 

 * **system**: This access policy is for high privilege actions that are offered by the API, such as changing the identity of an application pool to _LocalSystem_.

Each access policy has a set of requirements that can be configured. The available requirements are:

**users**: Specifies which roles from the _security.users_ section are allowed access. To allow all users use a value of '_Everyone_'.

**access_key**: Specifies whether requests are required to have an access token.

**read_only**: Enforces a read-only mode by restricting all requests to use the HTTP _GET_ method.

**forbidden**: Blocks all access.


## Complete Example

```
{
  "host_id": "",

  "host_name": "My instance of the IIS Administration API",

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

  "security": {
    "require_windows_authentication": true,
    "users": {
      "administrators": [
      ],
      "owners": [
      ]
    },
    "access_policy": {
      "api": {
        "users": "administrators",
        "access_key": true
      },
      "api_keys": {
        "users": "administrators",
        "access_key": false
      },
      "system": {
        "users": "owners",
        "access_key": true
      }
    }
  },

  "cors": {
    "rules": [
      {
        "origin": "https://contoso.com",
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
          "read"
        ]
      }
    ]
  }
}
```
