# Microsoft IIS Administration API

The Microsoft IIS Administration API is a REST API that allows consumers to configure and monitor their IIS web servers. With the API installed on your IIS machine, you can configure your IIS instance with any HTTP client including the web management tool at https://manage.iis.net.

## Whats New

This API allows IIS to be more open than ever by enabling IIS to be configured by anything that speaks HTTP. Previously users had to be on Windows machines to do any configuration or monitoring of IIS, but with this API even mobile devices are treated equally. Having this HTTP endpoint for IIS allows web based GUIs to take advantage, which is exactly what we're doing with the web based manager at https://manage.iis.net.