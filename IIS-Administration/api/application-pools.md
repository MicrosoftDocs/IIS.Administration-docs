---
uid: api/application-pools
---

# The Application Pool Resource

Application pools provide an isolation mechanism for processes on the web server. There are many different settings available to fine-tune the behavior of the worker processes used to serve request through IIS. The application pool API allows consumers to create, read, delete, or update their application pools.

**GET** _/api/webserver/application-pools/{id}_
```
{
    "name": "DefaultAppPool",
    "id": "{id}",
    "status": "started",
    "auto_start": "true",
    "pipeline_mode": "integrated",
    "managed_runtime_version": "v4.0",
    "enable_32bit_win64": "false",
    "queue_length": "1000",
    "cpu": {
        "limit": "0",
        "limit_interval": "5",
        "action": "NoAction",
        "processor_affinity_enabled": "false",
        "processor_affinity_mask32": "0xFFFFFFFF",
        "processor_affinity_mask64": "0xFFFFFFFF"
    },
    "process_model": {
        "idle_timeout": "20",
        "max_processes": "1",
        "pinging_enabled": "true",
        "ping_interval": "30",
        "ping_response_time": "90",
        "shutdown_time_limit": "90",
        "startup_time_limit": "90",
        "idle_timeout_action": "Terminate"
    },
    "identity": {
        "identity_type": "ApplicationPoolIdentity",
        "username": "",
        "load_user_profile": "true"
    },
    "recycling": {
        "disable_overlapped_recycle": "false",
        "disable_recycle_on_config_change": "false",
        "log_events": {
            "time": "true",
            "requests": "true",
            "schedule": "true",
            "memory": "true",
            "isapi_unhealthy": "true",
            "on_demand": "true",
            "config_change": "true",
            "private_memory": "true"
        },
        "periodic_restart": {
            "time_interval": "1740",
            "private_memory": "0",
            "request_limit": "0",
            "virtual_memory": "0",
            "schedule": []
        }
    },
    "rapid_fail_protection": {
        "enabled": "true",
        "load_balancer_capabilities": "HttpLevel",
        "interval": "5",
        "max_crashes": "5",
        "auto_shutdown_exe": "",
        "auto_shutdown_params": ""
    },
    "process_orphaning": {
        "enabled": "false",
        "orphan_action_exe": "",
        "orphan_action_params": ""
    },
    "_links": {
        "webapps": {
            "href": "/api/webserver/webapps?application_pool.id={id}"
        },
        "websites": {
            "href": "/api/webserver/websites?application_pool.id={id}"
        },
        "worker_processes": {
            "href": "/api/webserver/worker-processes?application_pool.id={id}"
        }
    }
}
```

## Creating an Application pool

The only information required to create an application pool is a name.

Creating an application pool. **POST** _/api/application-pools_
```
{
    "name": "Demonstration App Pool"
}
```