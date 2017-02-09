---
uid: introduction
---

# Introduction to the Microsoft IIS Administration API

The Microsoft IIS Administration API is a REST API that enables consumers to configure and monitor their IIS web servers. With the API installed on an IIS machine, one can configure an IIS instance with any HTTP client including the web management tool at https://manage.iis.net. 

### Why Use IIS Administration?

There are many methods available to configure IIS including appcmd.exe, PowerShell, and .NET. These methods have their benefits, but one thing they lack is an open and standard interface. The IIS Administration API builds upon the principles of REST APIs to provide an interface that can be consumed regardless of platform. This is the ultimate way to open up IIS to any client. There are few frameworks today that don't provide HTTP support, and most frameworks provide methods to simplify communicating with REST APIs. Powerful scripts can be made from a myriad of clients such as PowerShell, cURL, and Python just by performing HTTP requests with JSON payloads.

IIS Administration is the perfect option for remote management. The API is a micro service that runs on the target machine, so by nature it is meant to serve remote requests. Opening up the port that the service listens on is the only step necessary to make remote management available for the machine. This means, web site creation, application pool monitoring, and security configuration can be handled from one client for a cluster of machines with little overhead.

The IIS Administration service is being developed to completely open up IIS. There are constantly features being worked on and added. We are able to release updates quickly because the Administration Service is a separate download from the IIS service. This also has means that customers can depend on whichever version of the API that they choose.

Security is a top priority. Our service can use any form of authentication and authorization that IIS uses. This means client certificate authentication, basic authentication, and even Windows authentication. Every call made to the API requires an access token in the request header and the service is not available from outside of the machine unless the port that it listens on is opened. The built-in security allows users to confidently install the IIS Administration API on their machines without worrying that they are compromising their system.

### So Many Options

 A REST API is accessible from anywhere. 
 * PowerShell
 * .NET
 * Java
 * Python
 * Javascript
 * Native mobile applications (Android, iOS, Windows)
 * Browser applications

A great example of what the IIS Administration API enables is the web UI that we have developed at [manage.iis.net](https://manage.iis.net)