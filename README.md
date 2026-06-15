# StatsD (statsd)

StatsD is the network daemon and UDP-based line protocol for application metrics, originally written at Etsy and now maintained at github.com/statsd/statsd. A StatsD client emits short text packets — counters, gauges, timers, histograms, sets, and meters — to a daemon that aggregates them in memory and flushes derived series to a backend such as Graphite, InfluxDB, Datadog, CloudWatch, or any of 30+ third-party sinks. The wire format is the canonical contract; dozens of language clients and several alternative servers (DogStatsD, Telegraf, gostatsd, brubeck, statsite, Veneur, bioyino) speak it with small, well-documented dialect extensions for tags, histograms, events, and service checks.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/statsd/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/statsd/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Aggregation
- Daemon
- DogStatsD
- Line Protocol
- Metrics
- Observability
- Open Source
- StatsD
- TCP
- UDP
- Wire Protocol

## Timestamps

- **Created:** 2024-01-01
- **Modified:** 2026-05-23

## APIs

### StatsD Wire Protocol

The StatsD wire protocol is a UTF-8, line-oriented, UDP-by-default packet format used to push application metrics from clients to a StatsD daemon. Each line takes the form `bucket:value|type[|@sample_rate]`, where `type` is one of `c` (counter), `g` (gauge, with optional `+`/`-` for delta), `ms` (timer), `h` (histogram), `s` (set), or `m` (meter, in some dialects). Multiple metrics may share a packet when separated by `\n`. The protocol is connectionless on UDP (port 8125) and connection-oriented on TCP, where each metric MUST be terminated by `\n`. The wire format is the canonical contract between StatsD, its tag-aware dialects (DogStatsD, InfluxDB, SignalFx), and the wider ecosystem of clients and servers.

- **Human URL:** [https://github.com/statsd/statsd/blob/master/docs/metric_types.md](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)

#### Tags

- Counters
- Gauges
- Histograms
- Line Protocol
- Meters
- Metrics
- Sets
- Timers
- UDP
- Wire Protocol

#### Properties

- [Documentation](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)
- [Protocol Spec](https://github.com/b/statsd_spec)
- [GitHub Repository](https://github.com/statsd/statsd)
- [AsyncAPI](asyncapi/statsd-wire-protocol-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)
- [JSON Schema](json-schema/statsd-metric-line-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/statsd-metric-instance-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Structure](json-structure/statsd-metric-instance-structure.json)
- [JSON-LD](json-ld/statsd-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Vocabulary](vocabulary/statsd-vocabulary.yml)
- [Postman Collection](collections/statsd-admin-interface.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/statsd-admin-interface.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### StatsD Admin Interface

The StatsD admin/management interface is a plain-text TCP service exposed on port 8126 (default), used to inspect daemon state and clear in-memory aggregates. Supported commands are `stats`, `counters`, `gauges`, `timers`, `delcounters`, `delgauges`, `deltimers`, `health [up|down]`, `config`, and `quit`. Each command is line-terminated; the daemon responds with one or more lines of plain text and the command name as a terminator (`END`). Wildcards (`*`) are supported on the delete commands. The interface is intended for telnet/netcat-style operator use and for tooling that wants to snapshot counters or toggle health checks.

- **Human URL:** [https://github.com/statsd/statsd/blob/master/docs/admin_interface.md](https://github.com/statsd/statsd/blob/master/docs/admin_interface.md)

#### Tags

- Admin
- Health Check
- Management
- Operator
- TCP

#### Properties

- [Documentation](https://github.com/statsd/statsd/blob/master/docs/admin_interface.md)
- [OpenAPI](openapi/statsd-admin-interface-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/statsd-admin-interface.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/statsd-admin-interface.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Spectral Rules](rules/statsd-admin-interface-rules.yml) — [Spectral](https://docs.stoplight.io/docs/spectral)
- [Examples](examples/statsd-admin-stats-example.json)

### DogStatsD Wire Protocol

DogStatsD is the Datadog Agent's StatsD-compatible ingestion protocol. It is a strict superset of the StatsD wire format that adds first-class tag syntax (`|#k:v,k:v`), histogram (`|h`) and distribution (`|d`) metric types, events (`_e{title.len,text.len}:title|text|...`), service checks (`_sc|name|status|...`), and Unix Domain Socket transport in addition to UDP/8125. Wide adoption beyond Datadog itself — Stripe Veneur, Atlassian gostatsd, Telegraf, and Shopify's statsd-instrument all speak DogStatsD — has made it the de facto modern dialect of the StatsD line protocol.

- **Human URL:** [https://github.com/statsd/statsd/blob/master/docs/metric_types.md](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)

#### Tags

- Datadog
- Distributions
- DogStatsD
- Events
- Histograms
- Service Checks
- Tags
- Unix Domain Socket
- Wire Protocol

#### Properties

- [Documentation](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)
- [AsyncAPI](asyncapi/dogstatsd-wire-protocol-asyncapi.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)
- [JSON Schema](json-schema/dogstatsd-event-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/dogstatsd-service-check-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [Examples](examples/dogstatsd-tagged-counter-example.json)
- [Examples](examples/dogstatsd-distribution-example.json)
- [Examples](examples/dogstatsd-event-example.json)
- [Examples](examples/dogstatsd-service-check-example.json)
- [Postman Collection](collections/statsd-admin-interface.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/statsd-admin-interface.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Git Hub](https://github.com/statsd/statsd)
- [License](https://github.com/statsd/statsd/blob/master/LICENSE)
- [Changelog](https://github.com/statsd/statsd/blob/master/CHANGELOG.md)
- [Contributor Guide](https://github.com/statsd/statsd/blob/master/CONTRIBUTING.md)
- [Wiki](https://github.com/statsd/statsd/wiki)
- [Protocol Spec](https://github.com/b/statsd_spec)
- [Package Registry](https://www.npmjs.com/package/statsd)
- [Issues](https://github.com/statsd/statsd/issues)
- [SDK](https://github.com/msiebuhr/node-statsd-client)
- [SDK](https://github.com/jsocol/pystatsd)
- [SDK](https://github.com/sivy/py-statsd)
- [SDK](https://github.com/WoLpH/python-statsd)
- [SDK](https://github.com/WoLpH/django-statsd)
- [SDK](https://github.com/Shopify/statsd-instrument)
- [SDK](https://github.com/reinh/statsd)
- [SDK](https://github.com/youdevise/java-statsd-client)
- [SDK](https://github.com/flozano/statsd-netty)
- [SDK](https://github.com/smira/go-statsd)
- [SDK](https://github.com/cactus/go-statsd-client)
- [SDK](https://github.com/quipo/statsd)
- [SDK](https://github.com/thephpleague/statsd)
- [SDK](https://github.com/domnikl/statsd-php)
- [SDK](https://github.com/justeat/JustEat.StatsD)
- [SDK](https://github.com/peschuster/graphite-client)
- [SDK](https://github.com/cosimo/perl5-net-statsd)
- [SDK](https://github.com/sanbeg/Etsy-Statsd)
- [SDK](https://metacpan.org/pod/Metrics::Any::Adapter::Statsd)
- [SDK](https://github.com/DataDog/datadogpy)
- [Alternative Server](https://docs.datadoghq.com/developers/dogstatsd/)
- [Alternative Server](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/statsd)
- [Alternative Server](https://github.com/stripe/veneur)
- [Alternative Server](https://github.com/atlassian/gostatsd)
- [Alternative Server](https://github.com/github/brubeck)
- [Alternative Server](https://github.com/armon/statsite)
- [Alternative Server](https://github.com/jbuchbinder/statsd-c)
- [Alternative Server](https://github.com/wayfair/statsdcc)
- [Alternative Server](https://github.com/avito-tech/bioyino)
- [Alternative Server](https://github.com/bitly/statsdaemon)
- [Alternative Server](https://github.com/vimeo/statsdaemon)
- [Alternative Server](https://github.com/amir/gographite)
- [Alternative Server](https://github.com/netdata/netdata)
- [Backend](https://github.com/statsd/statsd/blob/master/docs/graphite.md)
- [Backend](https://github.com/statsd/statsd/blob/master/docs/backend.md)
- [Backend](https://github.com/statsd/statsd/blob/master/docs/backend.md)
- [Backend](https://github.com/camitz/aws-cloudwatch-statsd-backend)
- [Backend](https://github.com/bernd/statsd-influxdb-backend)
- [Backend](https://github.com/DataDog/statsd-datadog-backend)
- [Backend](https://github.com/emurphy/statsd-opentsdb-backend)
- [Backend](https://github.com/jjneely/statsd-stackdriver-backend)
- [Backend](https://github.com/markkimsal/statsd-elasticsearch-backend)
- [Integrations](https://graphiteapp.org/)
- [Integrations](https://www.datadoghq.com/)
- [Integrations](https://www.influxdata.com/)
- [Integrations](https://aws.amazon.com/cloudwatch/)
- [Integrations](https://www.splunk.com/en_us/products/observability.html)
