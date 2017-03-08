---
uid: api/request-tracing
---

# HTTP Request Tracing

HTTP request tracing is a feature of IIS that provides a way to determine what exactly is happening with a request. This includes any form of authentication, which handler was used, and how long each step took in the pipeline. Enabling request tracing is a useful way to diagnose unexpected or undesirable behavior.

## Feature Settings (/api/webserver/http-request-tracing)

The HTTP request tracing feature creates trace files based on a configured set of rules. The information in the trace files is determined by what providers are avaialable for that rule. The feature settings for request tracing deal with the generation of the trace files.

**GET** _/api/webserver/http-request-tracing/{request-tracing-id}_
```
{
    "id": "{request-tracing-id}",
    "scope": "",
    "metadata": {
        "is_local": "true",
        "is_locked": "false",
        "override_mode": "inherit",
        "override_mode_effective": "allow"
    },
    "enabled": "true",
    "directory": "%SystemDrive%\\inetpub\\logs\\FailedReqLogFiles",
    "maximum_number_trace_files": "50",
    "website": null,
    "_links": {
        "providers": {
            "href": "/api/webserver/http-request-tracing/providers?http_request_tracing.id={request-tracing-id}"
        },
        "rules": {
            "href": "/api/webserver/http-request-tracing/rules?http_request_tracing.id={request-tracing-id}"
        }
    }
}
```

## Trace Providers (/api/webserver/http-request-tracing/providers)

The providers for HTTP request tracing determine what information will be provided whenever a trace rule is triggered. IIS comes with a set of default providers that provide most of the information a consumer will want.

**GET** _/api/webserver/http-request-tracing/providers/{provider-id}_
```
{
    "name": "ASPNET",
    "id": "{provider-id}",
    "guid": "{aff081fe-0247-4275-9c4e-021f3dc1da35}",
    "areas": [
        "Infrastructure",
        "Module",
        "Page",
        "AppServices"
    ],
    "request_tracing": {
        "id": "{request-tracing-id}",
        "scope": "",
        "_links": {
            "self": {
                "href": "/api/webserver/http-request-tracing/{request-tracing-id}"
            }
        }
    }
}
```

## Trace Rules (/api/webserver/http-request-tracing/rules)

Trace rules generate request tracing logs whenever a request is executed that meets the conditions specified in the trace rule. Trace rules can trigger based on status code, execution time, and path.

**GET** _/api/webserver/http-request-tracing/rules/{rule-id}_
```
{
    "path": "*",
    "id": "{rule-id}",
    "status_codes": [
        "101-999"
    ],
    "min_request_execution_time": "2147483647",
    "event_severity": "ignore",
    "custom_action": {
        "executable": "",
        "params": "",
        "trigger_limit": "1"
    },
    "traces": [
        {
            "allowed_areas": {
                "Authentication": "true",
                "Security": "true",
                "Filter": "true",
                "StaticFile": "true",
                "CGI": "true",
                "Compression": "true",
                "Cache": "true",
                "RequestNotifications": "true",
                "Module": "true",
                "FastCGI": "true",
                "WebSocket": "true"
            },
            "provider": {
                "name": "WWW Server",
                "id": "{provider-id}",
                "_links": {
                    "self": {
                        "href": "/api/webserver/http-request-tracing/providers/{provider-id}"
                    }
                }
            },
            "verbosity": "warning"
        }
    ],
    "request_tracing": {
        "id": "{request-tracing-id}",
        "scope": "",
        "_links": {
            "self": {
                "href": "/api/webserver/http-request-tracing/{request-tracing-id}"
            }
        }
    }
}
```

### Creating a Request Tracing Rule

A request tracing rule must specify which request tracing section it belongs to when it is being created, and also should specify any providers that should log information for the generated log file. In this example a rule is created that only generates trace logs for requests to _index.html_. The logs will include information from the _WWW Server_ trace provider.

POST _/api/webserver/http-request-tracing/rules_
```
{
    "path": "index.html",
    "status_codes": [
        "100-999"
    ],
    "traces": [
        {
            "allowed_areas": {
                "Authentication": "true",
                "Security": "true",
                "Filter": "true",
                "StaticFile": "true",
                "CGI": "true",
                "Compression": "true",
                "Cache": "true",
                "RequestNotifications": "true",
                "Module": "true",
                "FastCGI": "true",
                "WebSocket": "true"
            },
            "provider": {
                "id": "{www-server-provider-id}"
            },
            "verbosity": "warning"
        }
    ],
    "request_tracing": {
        "id": "{request-tracing-id}"
    }
}
```

## Trace Logs (/api/webserver/http-request-tracing/traces)

The Microsoft IIS Administration API provides an endpoint to view data for the trace logs that have been generated. This information helps quickly determine which trace file is of interest instead of having to open each file individually.

**GET** _/api/webserver/http-request-tracing/traces/{trace-id}_
```
{
    "id": "{trace-id}",
    "url": "http://localhost:80/favicon.ico",
    "method": "GET",
    "status_code": "404",
    "date": "2017-03-02T15:34:17.0627155-08:00",
    "time_taken": "0",
    "process_id": "8172",
    "activity_id": "{8000009D-0001-F700-B63F-84710C7967BB}",
    "file_info": {
        "name": "fr000001.xml",
        "id": "{file-id}",
        "type": "file",
        "physical_path": "c:\\inetpub\\logs\\FailedReqLogFiles\\W3SVC1\\fr000001.xml",
        "_links": {
            "self": {
                "href": "/api/files/{file-id}"
            }
        }
    },
    "request_tracing": {
        "id": "{request-tracing-id}",
        "scope": "Default Web Site/",
        "_links": {
            "self": {
                "href": "/api/webserver/http-request-tracing/{request-tracing-id}"
            }
        }
    }
}
```