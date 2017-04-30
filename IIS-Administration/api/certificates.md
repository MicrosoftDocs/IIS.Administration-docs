---
uid: api/certificates
---

# The Certificates Resource

Certificates provide the mechanism for web servers to prove their identity as well as secure communication channels. The certificates API is essential to creating web sites that serve content over HTTPS.

**GET** _/api/certificates/{id}_

```
{
    "alias": "My Self Signed Certificate",
    "id": "{id}",
    "issued_by": "CN=localhost",
    "subject": "CN=localhost",
    "thumbprint": "1E927A29E966FA11A7C469BC565A9E00B11F5F95",
    "signature_algorithm": "sha256RSA",
    "valid_from": "2017-04-12T11:26:26Z",
    "valid_to": "2019-04-12T11:26:26Z",
    "version": "3",
    "intended_purposes": [
        "Client Authentication",
        "Server Authentication"
    ],
    "private_key": {
        "exportable": "false"
    },
    "subject_alternative_names": [
        "DNS Name=localhost",
        "DNS Name=my-work-pc"
    ],
    "store": {
        "name": "My",
        "id": "{store-id}",
        "_links": {
            "self": {
                "href": "/api/certificates/stores/{store-id}"
            }
        }
    }
}
```

## Range Requests

It is not uncommon for a web server to have a very large amount of certificates. To improve usability of the certificates API for servers with a large amount of certificates, the endpoint supports range requests. Sending a HEAD request to the certificates endpoint will prompt the server to respond with the total number of certificates that are available. The certificates can then be requested in chunks by setting the *Range* header in subsequent requests.

*Range* request for a second set of 50 certificates
```
Range: certificates=50-99
```

# Certificate stores

All certificates belong to a certificate store. The certificate stores that are visible through the API can be retreived at the certificate stores endpoint _/api/certificates/stores_. This list of stores can be configured in the [application settings](../configuration/appsettings.json.md) files. These certificate stores have a _claims_ property which specify what operations are allowed on the certificates inside the store. While currently reading is the only available operation, importing, exporting, deleting, and creating certificates are planned.

**GET** _/api/certificates/stores_

```
{
    "stores": [
        {
            "name": "My",
            "id": "{store-id}",
            "_links": {
                "self": {
                    "href": "/api/certificates/stores/{store-id}"
                }
            }
        },
        {
            "name": "WebHosting",
            "id": "{store-id}"
            // _links omitted
        },
        {
            "name": "IIS Central Certificate Store",
            "id": "{store-id}"
            // _links omitted
        }
    ]
}
```

## Listing Certificates for a Specific Store

The certificates API supports filtering certificates by store. To do this, the _id_ property of the target certificate store should be retreived. This can be done through the certificate stores endpoint. Then a request should be made to the certificates endpoint that specifies the _store.id_ field in the query string. The following request will retreive all certificates for the built in _Web Hosting_ certificate store.

*GET* _/api/certificates?store.id={web-hosting-store-id}_

```
{
    "certificates": [
        {
            "alias": "WebHostCert",
            "id": "{cert-id}",
            "issued_by": "CN=localhost",
            "subject": "CN=localhost",
            "thumbprint": "8E7933F41998C507B30F0E0AC8B548A903FE7843",
            "valid_to": "2019-02-28T14:32:33Z",
            "_links": {
                "self": {
                    "href": "/api/certificates/{cert-id}"
                }
            }
        }
    ]
}

```