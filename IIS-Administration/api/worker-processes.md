---
uid: api/worker-processes
---

# The Worker Process Resource

Worker processes provide the execution environment for all web sites and applications configured in IIS. Valuable information such as CPU utilization and memory footprint can be obtained from the API to help monitor the health of worker processes and the web server. The _/api/webserver/worker-processes_ endpoint lists all the worker processes that are currently running.

**GET** _/api/webserver/worker-processes/{worker-process-id}_
```
{
    "name": "w3wp",
    "id": "{worker-process-id}",
    "status": "running",
    "process_id": "45076",
    "process_guid": "63e9cb86-592d-4080-9132-5a9bec85d7c3",
    "start_time": "2017-03-08T09:42:34.9696447-08:00",
    "working_set": "43098112",
    "peak_working_set": "43098112",
    "private_memory_size": "118493184",
    "virtual_memory_size": "2215549431808",
    "peak_virtual_memory_size": "2215550480384",
    "total_processor_time": "00:00:00.2812500",
    "application_pool": {
        "name": "DefaultAppPool",
        "id": "{app-pool-id}",
        "status": "started"
    },
    "_links": {
        "request_monitor": {
            "href": "/api/webserver/http-request-monitor/requests?wp.id={worker-process-id}"
        }
    }
}
```

## Filtering by Application Pool

The worker processes that are running for a given application pool can be obtained by specifying the application pool's id at the worker processes endpoint.

**GET** _/api/webserver/worker-processes?application_pool.id={application-pool-id}_
```
{
    "worker_processes": [
        {
            "name": "w3wp",
            "id": "{worker-process-id}",
            "process_id": "45076"
        }
    ]
}
```

## Terminating A Worker Process

The API supports the ability to terminate a worker process by sending a DELETE request to the worker processes endpoint at _/api/webserver/worker-processes/{worker-process-id}_