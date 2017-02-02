---
uid: management-portal/connecting
---

# Connecting to the Management Portal

The web based manager at [manage.iis.net](https://manage.iis.net) is the perfect way to administer IIS machines. The web manager can be used to connect to multiple servers whether they be local or remote. The portal can even be used to download and install the API if it is not already present on the local machine.

## Initial Visit
When visiting the management portal for the first time, a welcome page will appear allowing the API to be downloaded. The download button contains the latest release of the IIS Administration API. Once the installer has been downloaded the management portal will wait for the installation to finish. To skip installation of the API and go straight to the connect screen click the _Skip this_ button.

![Initial visit to https://manage.iis.net][get]

![Waiting on installation to finish][await]

## Connecting

When the installation has completed successfully the connect screen will appear. From this screen, connections can be made to any IIS server that is running the administration API. To finish connecting to the local instance of the API, an access token must be acquired.  After generating an access token, return to the connect screen and paste the access token into the Access Token field. Click connect to finish creating the connection to the API.

![Finished installing Microsoft IIS Administration API][connect]

### Acquiring an Access Token

Clicking the "Get Access Token" link under the Access Token field will open up the [access tokens](../security/access-tokens.md) page of the [API explorer](../api-explorer/index.md). Once at the access key creation screen, create a key to enable access to the API from the management portal. When an access key is generated the expiration time of the key can be set and a purpose can be provided. A good purpose will tell the reader who the key belongs to, where it is being used, and how it is being used. The creation of an access key will display an associated _access token_. This is the private value that is used on the GUI connection screen and it should not be shared.

![Generation screen for access tokens][generation]

![Freshly generated access token][generated]

### Connect

When the access token has been provided in the access token field, the connect button will light up. Check the _remember me_ box if using a trusted machine to make the management experience better on subsequent accesses. Otherwise the access token will have to be input again. Click the connect button to move to the homepage of the web manager.

![Entering the access token into the access token field][entered]

![The view after completing a connection][finished]

## Changing the Current connection

If the _remember me_ box is checked when making a connection then the connection is saved. The active connection can be changed at any time by clicking the connection menu at the top left. The connection menu lists all the saved connections. It can also be used to edit or delete an existing connection. Select the connection out of the menu to make it the active connection.

![Changing the active connection][change]


[get]: _static/manage.iis.net-get.png "Welcome screen at https://manage.iis.net"
[await]: _static/manage.iis.net-await.png "Waiting on installation to finish"
[connect]: _static/manage.iis.net-connect.png "Finished installing Microsoft IIS Administration API"
[generation]: _static/access-token-generation.png "Generation screen for access tokens"
[generated]: _static/access-token-generated.png "Freshly generated access token"
[entered]: _static/access-token-entered.png "Entering the access token into the access token field"
[finished]: _static/finished_connecting.png "The view after completing a connection"
[change]: _static/changing-connection.png "Changing the active connection"