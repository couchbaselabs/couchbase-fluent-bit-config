# Fluent Bit Configuration for Couchbase Logs

<p align="center">

  <a href="https://github.com/couchbaselabs/couchbase-fluent-bit-config/graphs/contributors" alt="Contributors">
    <img src="https://img.shields.io/github/license/couchbaselabs/couchbase-fluent-bit-config" />
  </a>

  <!--
  <a href="https://galaxy.ansible.com/couchbaselabs/exporter" alt="Ansible Role">
    <img src="https://img.shields.io/ansible/role/{roleId}" />
  </a>

  <a href="https://galaxy.ansible.com/couchbaselabs/exporter" alt="Ansible Quality Score">
    <img src="https://img.shields.io/ansible/quality/{roleId}" />
  </a>

  <a href="https://galaxy.ansible.com/couchbaselabs/exporter" alt="Downloads">
    <img src="https://img.shields.io/ansible/role/d/{roleId}" />
  </a>
  -->

  <a href="https://github.com/couchbaselabs/couchbase-fluent-bit-config/releases" alt="GitHub tag (latest by date)">
    <img src="https://img.shields.io/github/v/tag/couchbaselabs/couchbase-fluent-bit-config" />
  </a>

  <a href="https://github.com/couchbaselabs/couchbase-fluent-bit-config/actions" alt="GitHub Workflow Status">
    <img src="https://img.shields.io/github/workflow/status/couchbaselabs/couchbase-fluent-bit-config/Lint" />
  </a>

  <a href="https://github.com/couchbaselabs/couchbase-fluent-bit-config/commits/main" alt="GitHub last commit">
    <img src="https://img.shields.io/github/last-commit/couchbaselabs/couchbase-fluent-bit-config" />
  </a>

  <a href="https://github.com/couchbaselabs/couchbase-fluent-bit-config/graphs/contributors" alt="GitHub last commit">
    <img src="https://img.shields.io/github/contributors/couchbaselabs/couchbase-fluent-bit-config" />
  </a>

</p>

## Description

A common [Fluent Bit](https://fluentbit.io) Configuration for  log parsing and shipping of [Couchbase Server](https://www.couchbase.com) log files and slow queries.  Projects using this configuration:

-   [couchbaselabs.couchbase_fluent_bit](https://github.com/couchbaselabs/ansible-couchbase-fluent-bit) Ansible Role
-   [couchbase-fluent-bit](https://github.com/couchbase/couchbase-fluent-bit) Sidecar used with the [Couchbase Autonomous Operator](https://www.couchbase.com/products/cloud/kubernetes)


## Disclaimer

This configuration files and the sub-components configured are not officially supported under Couchbase Enterprise Subscriptions. Please contact Couchbase on any details for your particular environment. Contents here and sub-components are provided as is, it is maintained through community contributions.  Any issues should be reported as an [Issue](https://github.com/couchbaselabs/couchbase-fluent-bit-config/issues) on Github.  Pull Requests are welcomed!

### Log File Inputs

| Log File       | Parser        |
| -------------- | ------------- |
| `analytics.log`            |  [`in-analytics.conf`](./conf/couchbase/input/in-analytics.conf) |
| `audit.log`                |  [`in-audit-log.conf`](./conf/couchbase/input/in-audit-log.conf) |
| `babysitter.log`           |  [`in-babysitter-log.conf`](./conf/couchbase/input/in-babysitter-log.conf) |
| `couchdb.log`              |  [`in-couchdb-log.conf`](./conf/couchbase/input/in-couchdb-log.conf) |
| `debug.log`                |  [`in-debug-log.conf`](./conf/couchbase/input/in-debug-log.conf) |
| `eventing.log`             |  [`in-eventing-log.conf`](./conf/couchbase/input/in-eventing-log.conf) |
| `fts.log`                  |  [`in-fts-log.conf`](./conf/couchbase/input/in-fts-log.conf) |
| `http_access.log`          |  [`in-http-log.conf`](./conf/couchbase/input/in-http-log.conf) |
| `http_access_internal.log` |  [`in-http-log.conf`](./conf/couchbase/input/in-http-log.conf) |
| `indexer.log`              |  [`in-indexer.conf`](./conf/couchbase/input/in-indexer.conf) |
| `json_rpc.log`             |  [`in-json_rpc-log.conf`](./conf/couchbase/input/in-json_rpc-log.conf) |
| `mapreduce_errors.log`     |  [`in-mapreduce_errors-log.conf`](./conf/couchbase/input/in-mapreduce_errors-log.conf) |
| `memcached.log`            |  [`in-memcached-log.conf`](./conf/couchbase/input/in-memcached-log.conf) |
| `metakv.log`               |  [`in-metakv-log.conf`](./conf/couchbase/input/in-metakv-log.conf) |
| `ns_couchdb.log`           |  [`in-ns_couchdb-log.conf`](./conf/couchbase/input/in-ns_couchdb-log.conf) |
| `projector.log`            |  [`in-projector.conf`](./conf/couchbase/input/in-projector.conf) |
| `prometheus.log`           |  [`in-prometheus-log.conf`](./conf/couchbase/input/in-prometheus-log.conf) |
| `query.log`                |  [`in-query-log.conf`](./conf/couchbase/input/in-query-log.conf) |
| `rebalance-report`         |  [`in-rebalance-report.conf`](./conf/couchbase/input/in-rebalance-report.conf) |
| `reports.log`              |  [`in-reports-log.conf`](./conf/couchbase/input/in-reports-log.conf) |
| `goxdcr.log`               |  [`in-xdcr-log.conf`](./conf/couchbase/input/in-xdcr-log.conf) |
| `slow_query.log`               |  [`in-slow-query-exec.conf`](./conf/couchbase/input/in-slow-query-exec.conf) |

There are no slow query logs by default in Couchbase Server, we leverage a [`query-logger`](./conf/couchbase/scripts/query-logger) shell script that is executed every 60s that will retrieve any new slow queries (>1000ms), which have been added to the `system:completed_requests` keyspace within the last 60s, if the node is a query node.  This requires that the [jq](https://stedolan.github.io/jq/) be installed.

### Environment Variables

| Environment Variable | Suggested Value | Description |
| -------------------- | --------------- | ----------- |
| `FLUENTBIT_VERSION`  | `` | The version of Fluent Bit that is Installed  |
| `FLUENT_BIT_LOG_LEVEL`  | `info` | The Fluent Bit Log Loevel |
| `STORAGE_BUFFER_PATH`  | `/var/lib/fluent-bit` | The filesystem location for Fluent Bit to buffer to if necessary  |
| `HTTP_PORT`  | `2020` | The port that Fluent Bit listens for requests on  |
| `COUCHBASE_FLUENTBIT_VERSION`  | `` |   |
| `COUCHBASE_CLUSTER_NAME`  | `` |  The name of the Couchbase Cluster |
| `COUCHBASE_SERVER_VERSION`  | `` | The Couchbase Server and Build Version |
| `COUCHBASE_LOGS`  | `/opt/couchbase/var/lib/couchbase/logs` | The location of the Couchbase logs directory  |
| `COUCHBASE_AUDIT_LOGS`  | `/opt/couchbase/var/lib/couchbase/logs` | The location of the Couchbase Audit logs directory  |
| `COUCHBASE_LOGS_REBALANCE_TMP_DIR`  | `/opt/couchbase/var/lib/couchbase/logs/rebalance` | The location of the Couchbase rebalance directory  |
| `COUCHBASE_NODE`  | `` | The hostname of the Couchbase Node as configured in the UI |
| `COUCHBASE_SERVICES`  | `` |  A comma-delimited list of services that are running on the node.  Values should be: kv,n1ql,index,cbas,fts,eventing,backup  |
| `MBL_AUDIT`  | `false` | Memory Buffer Limit for Audit log parsing, disabled by default.  |
| `MBL_ERLANG`  | `false` |  Memory Buffer Limit for Erlang logs parsing, disabled by default. |
| `MBL_EVENTING`  | `false` |  Memory Buffer Limit for Eventing log parsing, disabled by default. |
| `MBL_FTS`  | `false` |  Memory Buffer Limit for FTS log parsing, disabled by default. |
| `MBL_HTTP`  | `false` | Memory Buffer Limit for HTTP log parsing, disabled by default. |
| `MBL_INDEX`  | `false` | Memory Buffer Limit for Index log parsing, disabled by default.  |
| `MBL_PROJECTOR`  | `false` |  Memory Buffer Limit for Projector log parsing, disabled by default. |
| `MBL_QUERY`  | `false` | Memory Buffer Limit for Analytics Java log parsing, disabled by default.  |
| `MBL_JAVA`  | `false` |  Memory Buffer Limit for Memcached log parsing, disabled by default. |
| `MBL_MEMCACHED`  | `false` |  Memory Buffer Limit for Prometheus log parsing, disabled by default. |
| `MBL_PROMETHEUS`  | `false` | Memory Buffer Limit for Rebalance log parsing, disabled by default.  |
| `MBL_REBALANCE`  | `false` | Memory Buffer Limit for XDCR log parsing, disabled by default. |
| `MBL_XDCR`  | `false` |  Memory Buffer Limit for Query log parsing, disabled by default. |


#### Standard Out Environment Variables

| Environment Variable | Suggested Value | Description |
| -------------------- | --------------- | ----------- |
| `STDOUT_MATCH`  | `none.*` |  The tags to match for outputting to stdout.  This is useful for validating log messages, but it is advised to not output everything as this can fill up the system logs. |


### cbhealthagent Environment Variables

| Environment Variable | Suggested Value | Description |
| -------------------- | --------------- | ----------- |
| `CBHEALTHAGENT_MATCH`  | `couchbase.log.*,couchbase.kernal` | The tags to match for [cbhealthagent](https://docs.couchbase.com/cmos/current/tutorial-onpremise.html)  |
| `CBHEALTHAGENT_PORT`  | `9084` | The TCP port that [cbhealthagent](https://docs.couchbase.com/cmos/current/tutorial-onpremise.html) is listening on  |


### Loki Environment Variables

| Environment Variable | Suggested Value | Description |
| -------------------- | --------------- | ----------- |
| `LOKI_MATCH`  | `couchbase.log-level.WARN.*,couchbase.log-level.ERROR.*,couchbase.log-level.INFO.memcached,couchbase.log.slow_query,couchbase.audit.log` | The tags to match when sending to Loki.  |
| `LOKI_HOST`  | `` |  The Loki host address |
| `LOKI_PORT`  | `3100` | The Loki host port  |
| `LOKI_TENANT_ID`  | `` | Tenant ID used by default to push logs to Loki. If omitted or empty it assumes Loki is running in single-tenant mode and no X-Scope-OrgID header is sent.  |
| `LOKI_HTTP_USER`  | `` | Set HTTP basic authentication user name if authentication is enabled  |
| `LOKI_HTTP_PASSWD`  | `` | Set HTTP basic authentication password if authentication is enabled  |
| `LOKI_TLS`  | `off` |  Whether or not to use TLS  |
| `LOKI_TLS_VERIFY`  | `off` | Whether not TLS verification should be skipped  |
| `LOKI_WORKERS`  | `1` |  The number of worker threads to use for Loki output |

### Splunk Environment Variables

| Environment Variable | Suggested Value | Description |
| -------------------- | --------------- | ----------- |
| `SPLUNK_MATCH`  | `couchbase.log-level.WARN.*,couchbase.log-level.ERROR.*,couchbase.log-level.INFO.memcached,couchbase.log.slow_query,couchbase.audit.log` | The tags to match when sending to Splunk  |
| `SPLUNK_HOST`  | `` | The Splunk host address  |
| `SPLUNK_PORT`  | `8088` | The Splunk host port  |
| `SPLUNK_TOKEN`  | `` | The Splunk Token  |
| `SPLUNK_HTTP_USER`  | `` | Set HTTP basic authentication password if authentication is enabled  |
| `SPLUNK_HTTP_PASSWD`  | `` |  Set HTTP basic authentication user name if authentication is enabled  |
| `SPLUNK_TLS`  | `off` |  Whether or not to use TLS |
| `SPLUNK_TLS_VERIFY`  | `off` |  Whether not TLS verification should be skipped |
| `SPLUNK_COMPRESS`  | `null` | Whether or not to perform compression, the only valid value is `"gzip"`  |
| `SPLUNK_CHANNEL`  | `` |  The Splunk Channel to user |
| `SPLUNK_WORKERS`  | `2` |  The number of worker threads to use for Splunk output |

## Output Variables

The following output tags/patterns are available for matching:

-   `couchbase.log.<log-name>`
-   `couchbase.log-level.<log-level>.<log-name>`
-   `couchbase.audit.messages`
-   `couchbase.audit.bucket.<name-of-bucket>`


The following log levels  are available:

-   `DEBUG`
-   `INFO`
-   `WARN`
-   `ERROR`
-   `UNKNOWN`


The following log names are available:

-   analytics
-   audit
-   babysitter
-   couchdb
-   debug
-   eventing
-   fts
-   http
-   indexer
-   json_rpc
-   mapreduce_errors
-   memcached
-   metakv
-   ns_couchdb
-   projector
-   prometheus
-   query
-   rebalance-re
-   reports
-   slow_query
-   xdcr

The following is example tags:

Output All Logs

```bash
couchbase.log.*
```

Output only slow_query and audit logs

```bash
couchbase.log.slow_query,couchbase.log.audit
```

Output only Errors and Warnings

```bash
couchbase.log-levels.WARN.*,couchbase.log-levels.ERROR.*
```

Output Errors and Warnings, as well as slow_query and audit

```bash
couchbase.log-levels.WARN.*,couchbase.log-levels.ERROR.*,couchbase.log.slow_query,couchbase.log.audit
```

Consider the following match to ship all `WARN` and `ERROR` messages, along with all memcached.log, audit.log and slow_query.log messages.

```
couchbase.log-level.WARN.*,couchbase.log-level.ERROR.*,couchbase.log.slow_query,couchbase.audit.log
```

