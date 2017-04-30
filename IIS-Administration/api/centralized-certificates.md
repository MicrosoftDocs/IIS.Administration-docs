---
uid: api/centralized-certificates
---

# The Central Certificate Store Resource

The Central Certificate Store (CCS) feature of IIS provided a mechanism to place certificates in a file share for use by multiple web servers. This feature can be enabled, disabled, and configured through the CCS API _/api/webserver/centralized-certificates/{ccs-id}_.

## Requirements

To enable the Central Certificate Store feature, the API should have read access to the physical path of the central certificate store. This can be configured in the [application settings](../configuration/appsettings.json.md) files section. Attempting to set the path of the CCS feature to a physical path that is not allowed will result in a 403 Forbidden error.

## Checking if CCS is enabled

If the central certificate store feature is disabled, the ccs endpoint will return a _404 Feature Not Installed_ response.

*GET* _/api/webserver/centralized-certificates/{ccs-id}_, when CCS is not enabled

```
{
    "title": "Not found",
    "detail": "IIS feature not installed",
    "name": "IIS Central Certificate Store",
    "status": "404"
}
```

## Enabling CCS

To enable CCS, a POST request should be sent to the CCS API endpoint along with all the necessary data to enable the feature.

*POST* _/api/webserver/centralized-certificates/{ccs-id}_

```
{
    "path": "\\\\FileShare\\certs",
    "identity": {
        "username": "{Username of windows identity used to access file share}"
        "password": "{Password of windows identity used to access file share}"
    },
    "private_key_password": "{Password used to encrypt private keys}"
}
```

## Updating CCS

The Central Certificate Store settings can be updated by sending a PATCH request with the new settings.

## Disabling CCS

To disable the CCS feature, a DELETE request should be sent to the CCS endpoint _/api/webserver/centralized-certificates/{ccs-id}_.

## Reading CCS Certificates

When the central certificate store is enabled its certificates will be available through the [certificates](certificates.md) API. In order to populate the certificates API with the CCS certificates, the file share must allow READ access to the computer that the API is running on. This behavior is same as using file shares with the [files API](files.md#using-file-shares). 