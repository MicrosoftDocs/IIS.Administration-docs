---
title: API Explorer
description: The Microsoft IIS Administration API comes with a built in interface called the API Explorer.
ms.date: 10/30/2016
uid: api-explorer/index
---

# API Explorer
The Microsoft IIS Administration API comes with a built in interface called the API Explorer. This explorer allows users to navigate the API using hyperlinks. Resources can be created, retrieved, modified, and removed directly from this tool. By default the API Explorer can be reached by browsing https://localhost:55539 on any machine that has the API installed. This interface is also where [access tokens](../security/access-tokens.md) are created.


## Connecting

The first time that the api explorer is used it will prompt for an access token. Every request to the API requires an access token, even those made from the explorer. Input the access token that you use to communicate with the API or click on the "ACCESS KEYS" link at the top right of the explorer to generate an access token. Checking the "Keep me connected" box will allow your browser to remember your access token for future use of the explorer.

![Connecting to the API Explorer][explorer-connect]

## Browsing

Once you have connected to the explorer you can begin browsing. The API's resources have been designed using [HAL](http://stateless.co/hal_specification.html), which allows the resources to provide the navigation engine used by the explorer. As links are clicked the url in the nagivation pane at the top of the explorer is updated to reflect the location of the current resource.

![Browsing with the API Explorer][explorer]


[explorer-connect]: _static/explorer-connect.png "Connecting to the API Explorer"
[explorer]: _static/explorer.png "Browsing with the API Explorer"