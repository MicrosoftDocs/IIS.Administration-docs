---
title: Monitoring
description: The monitoring API provides performance and health data for the web server, web sites, and application pools.
ms.date: 10/30/2016
uid: api/monitoring
---

# Monitoring

The monitoring API provides performance and health data for the web server, web sites, and application pools. This data can be used to measure how effectively resources are being used. 

## Web Server Monitoring (/api/webserver/monitoring)

The _/api/webserver/monitoring_ endpoint exposes aggregrated performance data for the web server. These data points include networking, CPU, memory, HTTP requests, and caching. With the exception of some properties such as *system\_in\_use*, all data is scoped down to the web server. For instance, *private\_working\_set* only includes memory used by webserver worker processes.

### Example

Web Server Monitoring Resource
```
{
    "id": "{id}",
    "network": {
        "bytes_sent_sec": "59486",
        "bytes_recv_sec": "8313",
        "connection_attempts_sec": "0",
        "total_bytes_sent": "4797480792",
        "total_bytes_recv": "670503816",
        "total_connection_attempts": "1",
        "current_connections": "1"
    },
    "requests": {
        "active": "0",
        "per_sec": "64",
        "total": "5197703"
    },
    "memory": {
        "handles": "358",
        "private_bytes": "9097216",
        "private_working_set": "8032256",
        "system_in_use": "6734737408",
        "installed": "15403110400"
    },
    "cpu": {
        "threads": "24",
        "processes": "1",
        "percent_usage": "0",
        "system_percent_usage": "14"
    },
    "disk": {
        "io_write_operations_sec": "1",
        "io_read_operations_sec": "1",
        "page_faults_sec": "0"
    },
    "cache": {
        "file_cache_count": "2",
        "file_cache_memory_usage": "699",
        "file_cache_hits": "18506471",
        "file_cache_misses": "46266060",
        "total_files_cached": "10",
        "output_cache_count": "0",
        "output_cache_memory_usage": "0",
        "output_cache_hits": "0",
        "output_cache_misses": "18506478",
        "uri_cache_count": "2",
        "uri_cache_hits": "18506452",
        "uri_cache_misses": "26",
        "total_uris_cached": "13"
    }
}
```

### Field Explanations

**Network**

bytes\_sent\_sec: The number of bytes that the web server sent in the last second.

bytes\_recv\_sec: The number of bytes that the web server received in the last second.

connection\_attempts\_sec: The number of client connections that have been attempted in the last second.

total\_bytes\_sent: The number of bytes sent since the web server started.

total\_bytes\_recv: The number of bytes received since the web server started.

total\_connection\_attempts: The number of client connections that have been attempted since the web server started.

current\_connections: The number of active connections that are open on the web server.

**Requests**

active: The number of requests that are currently being processed by the web server.

per_sec: The number of requests that have been served in the past second.

total: The number of requests that have been served since the web server started.

**Memory**

handles: The number of handles that are currently open in web server processes.

private_bytes: The total private bytes being used by all web server processes.

private\_working\_set: The total private working set being used by all web server processes.

system\_in\_use: The total memory in use by the entire system.

installed: The total installed memory.

**CPU**

threads: The number of threads currently active in web server processes.

processes: The number of processes being used by the web server to process requests.

percent_usage: The percentage of CPU being used by web server processes.

system\_percent\_usage: The percentage of CPU being used by the entire system.

**Disk**

io\_write\_operations\_sec: The number of write operations performed by all webserver processes in the last second.

io\_read\_operations\_sec:  The number of read operations performed by all webserver processes in the last second.

page\_faults\_sec: The number of page faults experienced by all webserver processes in the last second.

**Cache**

file\_cache\_count: Current number of files whose content is in the user-mode cache.

file\_cache\_memory\_usage: Current number of bytes used for the user-mode file cache.

file\_cache\_hits: Number of successful lookups in the user-mode file cache since the web server started.

file\_cache\_misses: Number of unsuccessful look ups in the user-mode file cache since the web server started.

total\_files\_cached: Number of files whose content was ever added to the user-mode cache since the web server started.

output\_cache\_count: Current number of items is in the output cache.

output\_cache\_memory\_usage: Current number of bytes used for the output cache.

output\_cache\_hits: Number of successful lookups in the output cache since the web server started.

output\_cache\_misses: Number of unsuccessful lookups in the output cache since the web server started.

uri\_cache\_count: Number of URI information blocks that are currently in the user-mode cache.

uri\_cache\_hits: Number of successful look ups in the user-mode URI cache since the web server started.

uri\_cache\_misses: Number of unsuccessful look ups in the user-mode URI cache since the web server started.

total\_uris\_cached: Number of URI information blocks that have been added to the user-mode cache since the web server started.



## Web Site Monitoring (/api/webserver/websites/monitoring/{id})

The _/api/webserver/websites/monitoring_ endpoint exposes performance data for an individual web site. The data is similar to what is available from the web server monitoring resource.

**Note:** This data includes information from the application pool that the web site runs in. If multiple web sites are running in the same application pool, the web site data taken from the application pool may be invalid. Assign one site per application pool for accurate performance measurement at the web site level.

### Example

Web Site Monitoring Resource
```
{
    "id": "{id}",
    "uptime": "88170",
    "network": {
        "bytes_sent_sec": "31280",
        "bytes_recv_sec": "4371",
        "connection_attempts_sec": "0",
        "total_bytes_sent": "4939609870",
        "total_bytes_recv": "690368010",
        "total_connection_attempts": "1",
        "current_connections": "1"
    },
    "requests": {
        "active": "0",
        "per_sec": "33",
        "total": "5351689"
    },
    "memory": {
        "handles": "358",
        "private_bytes": "9097216",
        "private_working_set": "8032256",
        "system_in_use": "6704680960",
        "installed": "15403110400"
    },
    "cpu": {
        "percent_usage": "0",
        "threads": "24",
        "processes": "1"
    },
    "disk": {
        "io_write_operations_sec": "1",
        "io_read_operations_sec": "1",
        "page_faults_sec": "0"
    },
    "cache": {
        "file_cache_count": "2",
        "file_cache_memory_usage": "699",
        "file_cache_hits": "10703511",
        "file_cache_misses": "26758783",
        "total_files_cached": "2",
        "output_cache_count": "0",
        "output_cache_memory_usage": "0",
        "output_cache_hits": "0",
        "output_cache_misses": "10703512",
        "uri_cache_count": "2",
        "uri_cache_hits": "10703508",
        "uri_cache_misses": "4",
        "total_uris_cached": "2"
    },
    "website": {
        "name": "Default Web Site",
        "id": "{id}",
        "status": "started"
    }
}
```

### Field Explanations

Most of the fields are the same as the web server resource. A few additional fields are present as indicated below.

uptime: The number of seconds that have elapsed since the web site started.

website: The web site resource that the data belongs to.

## Application Pool Monitoring (/api/webserver/application-pools/monitoring/{id})

The _/api/webserver/application-pools/monitoring_ endpoint exposes performance data for an individual application pool. The data is similar to what is available from the web server monitoring resource. Some properties present in the web server are not available in the context of an application pool.

### Example

Application Pool Monitoring Resource
```
{
    "id": "{id}",
    "requests": {
        "active": "0",
        "per_sec": "76",
        "total": "5371766"
    },
    "memory": {
        "handles": "358",
        "private_bytes": "9097216",
        "private_working_set": "8032256",
        "system_in_use": "6742872064",
        "installed": "15403110400"
    },
    "cpu": {
        "percent_usage": "0",
        "threads": "24",
        "processes": "1"
    },
    "disk": {
        "io_write_operations_sec": "0",
        "io_read_operations_sec": "0",
        "page_faults_sec": "0"
    },
    "cache": {
        "file_cache_count": "2",
        "file_cache_memory_usage": "699",
        "file_cache_hits": "10743531",
        "file_cache_misses": "26858833",
        "total_files_cached": "2",
        "output_cache_count": "0",
        "output_cache_memory_usage": "0",
        "output_cache_hits": "0",
        "output_cache_misses": "10743532",
        "uri_cache_count": "2",
        "uri_cache_hits": "10743528",
        "uri_cache_misses": "4",
        "total_uris_cached": "2"
    },
    "application_pool": {
        "name": "DefaultAppPool",
        "id": "{id}",
        "status": "started"
    }
}
```