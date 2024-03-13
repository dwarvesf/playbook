# Monitoring
- [Monitoring](#monitoring)
- [Application monitoring](#application-monitoring)
  - [Status page](#status-page)
  - [Status page format](#status-page-format)
    - [Plain format](#plain-format)
    - [JSON format](#json-format)
  - [HTTP status codes](#http-status-codes)
  - [Load balancer health checks](#load-balancer-health-checks)
  - [Access control](#access-control)

# Application monitoring
Monitoring the full status of a service requires that both OS-level and application-specific monitoring checks are performed. OS-level checks include, for example, CPU, disk or memory usage, running processes, open ports, etc. Application specific checks are, however, the most important from the point of view of the running service. These can be anything from "does this URL respond and return the HTTP status 200", to checking database connectivity, data consistency, and so on.

This section describes a way to implement the application-specific checks, which would make it easier to monitor the overall application health and give full control to the application developers to determine what checks are meaningful in the context of the concrete application.

In essence, the idea is to have a single endpoint (an application URL) that can give a good status overview of the entire application. This is implemented inside the application and requires work from the project team, but on the other hand, the project team is the one who can really define what is an OK state of the application and what is considered an ERROR state.

The application could implement any number of "subsystem" checks. For example,

* connection to the database is up
* data is in an consistent state (e.g. a list of items in a certain database table is meaningful)
* 3rd party services that the application integrates to are reachable
* ElasticSearch indexes are in a consistent state
* anything else that makes sense for the application

A combined overview status should be provided by the application, aggregating the information from the various subsystem checks. The idea is that an external monitoring system can track only this combined overview, so that the external monitoring does not need to be reconfigured when a new application check is added or modified. Moreover, the developers are the ones that can decide about what the overall status is based on regarding subsystem checks (i.e. which ones are critical, while ones are not, etc).

## Status page
All status checks SHOULD be accessible under `/healthz` URLs as follows:

* `/healthz` - the overall status page (mandatory)
* `/healthz/subsystem1` - a status check for speciffic subsystem (optional)
* ...

The main `/healthz` page should at a minimum give an overall status of the system, as described in the next section. This means that the main `/healthz` page should execute ALL subsystem checks and report the aggregated overall system status. It is up to the developers to decide how the overall system status is determined based on the subsystems. For example an `ERROR` state of some non-critical subsystem may only generate an overall `WARNING` status.

For performance reasons, some subsystem checks may be excluded from this overall `/healthz` page - for example, when the check causes higher resource usage, takes longer time to complete, etc. Overall, the main status page should be light enough so that it can be polled relatively often (every 1-3 minutes) and not cause too much load on the system. Subsystem checks that are excluded from the overall status check should have their own URLs, as shown above. Naturally, monitoring those would require modifications in the monitoring system configuration. To overcome this, a different approach can be taken: the application could perform the heavy subsystem checks in a background process at a rate that is acceptable and store the status internally. This would allow the main status page to reflect also these heavy checks (e.g. it would retrieve the last performed check status). This approach should be used, unless its implementation is too difficult.

## Status page format
We propose two alternative formats for the status pages - `plain` and `JSON`.

### Plain format
The plain format has one status per line in the form `key: value`. The key is a subsystem/check name and the value is the status value. The status value can be one of:

* `OK`
* `WARN Message`
* `ERROR Message`

where `Message` can be some meaningful text, that can help quickly identify the problem. The message is single line, without specified length restriction, but use common sense - e.g. probably should not be longer than 200 characters.

The main status page MUST have a key called `status` that shows the overall aggregated application status. The individual subsystem check status lines are optional. Subsystem status keys should have a suffix `_status`. Here are some examples:

When everything is ok:

```
status: OK
database_status: OK
elastic_search_status: OK
```

When some check is failing:

```
status: ERROR Database is not accessible
database_status: ERROR Connection failed
elastic_search_status: OK
```

Multiple failures at the same time:

```
status: ERROR failed subsystems: database, elasticsearch. For details see https://myapp.example.com/status
database_status: ERROR Connection failed
elastic_search_status: WARN Too few entries in index A.
```

In addition to the status lines, a status page can have non-status keys. For example, those can be showing some metrics (that may or may not be monitored). The additional keys must be prefixed with the subsystem name.

```
status: OK
database_status: OK
database_customers: 378
database_items: 8934748
elastic_search_status: OK
elastic_search_shards: 20
```

The overall status may naturally be based on some of the metrics:

```
status: WARN Too few items in database
database_status: WARN Too few customers in database
database_customers: 378
database_items: 1
elastic_search_status: OK
elastic_search_shards: 20
```

Subsystem checks that have their own URL (`/healthz/subsystemX`) should follow a similar format, having a mandatory key `status` and a number of optional additional keys. Example for e.g. `/healthz/database`:

```
status: OK
connection_pool: 30
latency: 2
```

### JSON format
The JSON format of the status pages can be often preferable, for example when the tooling or integration to other systems is easier to achieve via a common data format.

The status values follow the same format as described above - `OK`, `WARN Message` and `ERROR Message`.

The equivalent to the status key form the plain format is a `status` key in the root JSON object. Subsystems should use nested objects also having a mandatory `status` key. Here are some examples:

All is fine:

```json
{
    "status": "OK",
    "database": {
        "status": "OK"
    },
    "elastic_search": {
        "status": "OK"
    }
}
```

Status page with additional metrics:

```json
{
    "status": "OK",
    "uptime": 18234,
    "database": {
        "status": "OK",
        "connection_pool": 30
    },
    "elastic_search": {
        "status": "OK",
        "multinode": false
    }
}
```

Something failing:

```json
{
    "status": "ERROR Database is not accessible. See https://myapp.example.com/healthz for details.",
    "database": {
        "status": "ERROR Connection failed",
        "connection_timeout": 30
    },
    "elastic_search": {
        "status": "OK"
    }
}
```

## HTTP status codes
Whenever the overall application status is OK, the HTTP status code in the status page response MUST be set to 200 (OK). Otherwise a 5XX error code SHOULD be set. For example, code 500 (Internal Server Error) could be used. Optionally, non-critical WARN status may still respond with 200.

## Load balancer health checks
Often the application is running behind a load balaner. Load balancers typically can monitor application servers by polling a given URL. The health check is used so that the load balancer can stop routing traffic to the failing application servers.

The overall `/healthz` page is a good candidate for the load balancer health check URL. However, a separate dedicated status page for a load balancer health check provides an important benefit. Such a page can be fine-tuned for when the application is considered to be healthy from the load balancer's perspective. For example, an error in a subsystem may still be considered a critical error for the overall application status, but does not necessarily need to cause the application server to be removed from the load balancer pool. A good example is a 3rd party integration status check. The load balancer health check page should only return non-200 status code when the application instance must be considered non-operational.

The load balancer health check page should be placed at a `/status/healthz` URL. Depending on your load balancer, the format of that page may deviate from the overall status format described here. Some load balancers may even observe only the returned HTTP status code.

## Access control
The status pages may need proper authorization in place, especially in case they expose debugging information in status messages or application metrics. HTTP basic authentication or IP-based restrictions are usually good enough candidates to consider.
