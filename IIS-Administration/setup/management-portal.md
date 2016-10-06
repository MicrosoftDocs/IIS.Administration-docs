---
uid: setup/management-portal
---

# Management Portal Based Setup

The easiest way to install the Microsoft IIS Administration API is by visiting the web based manager at https://manage.iis.net. It is recommended to first install the dependencies mentioned in [requirements](./requirements.md). When visiting this site for the first time, a prompt will appear allowing the API to be downloaded. Once the setup installer has been downloaded the management portal will wait for the installation to finish. 

![Initial visit to https://manage.iis.net][get]

![Waiting on installation to finish][await]

![Finished installing Microsoft IIS Administration API][connect]

## Connecting

Once the installation has completed successfully the connect screen will appear. From this screen, connections can be made to any IIS server that is running the administration API. To finish connecting to the local instance of the API, an access token must be acquired. Clicking the "Get Access Token" link under the Access Token field will open up the [access tokens](../security/access-tokens.md) page of the newly installed [API explorer](../api-explorer/index.md). Once an access token has been generated you can exit out of the API explorer and return to the connect screen. Paste the access token into the Access Token field and click connect.





[get]: /imgs/manage.iis.net-get.png "Welcome screen at https://manage.iis.net"
[await]: /imgs/manage.iis.net-await.png "Waiting on installation to finish"
[connect]: /imgs/manage.iis.net-connect.png "Finished installing Microsoft IIS Administration API"