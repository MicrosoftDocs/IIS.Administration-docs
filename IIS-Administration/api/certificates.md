---
uid: api/certificates
---

# The Certificate Resource

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

It is not uncommon for a web server to have a very large amount of certificates. To improve usability of the certificates API for these servers, the endpoint supports range requests. Sending a HEAD request to the certificates endpoint will prompt the server to respond with the total number of certificates that are available in the _X-Total-Count_ HTTP header. The certificates can then be requested in chunks by setting the *Range* header in subsequent requests.


**HEAD** _/api/certificates_

```
200 OK
x-total-count: 100
```

Retreiving the second and third certificates out of 100

```
GET /api/certificates
Access-Token: Bearer {Access-Token}
Range: certificates=1-2
```

## Certificate stores

All certificates belong to a certificate store. A list of the available certificate stores can be retreived from the certificate stores endpoint _/api/certificates/stores_. These certificate stores have a _claims_ property which specify what operations are allowed on the certificates inside the store. This access is configurable via the certificates section of the [application settings](../configuration/appsettings.json.md) file. Currently certificate stores only support read operations. In the future the API may support importing, exporting, deleting, and creating certificates.

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

**GET** _/api/certificates?store.id={store-id}_

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