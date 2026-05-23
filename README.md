# StatsD (statsd)

StatsD is the network daemon and UDP-based line protocol for application metrics, originally written at Etsy and now maintained at [github.com/statsd/statsd](https://github.com/statsd/statsd). A StatsD client emits short text packets — counters, gauges, timers, histograms, sets, and meters — to a daemon that aggregates them in memory and flushes derived series to a backend such as Graphite, InfluxDB, Datadog, CloudWatch, or any of 30+ third-party sinks. The wire format is the canonical contract; dozens of language clients and several alternative servers (DogStatsD, Telegraf, gostatsd, brubeck, statsite, Veneur, bioyino) speak it with small, well-documented dialect extensions for tags, histograms, events, and service checks.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/statsd/refs/heads/main/apis.yml)

## Tags

- Aggregation, Daemon, DogStatsD, Line Protocol, Metrics, Observability, Open Source, StatsD, TCP, UDP, Wire Protocol

## Timestamps

- **Created:** 2024-01-01
- **Modified:** 2026-05-23

## APIs

### StatsD Wire Protocol
The StatsD wire protocol is a UTF-8, line-oriented, UDP-by-default packet format. Each line takes the form `bucket:value|type[|@sample_rate]`, where `type` is one of `c` (counter), `g` (gauge, with optional `+`/`-` for delta), `ms` (timer), `h` (histogram), `s` (set), or `m` (meter). Multiple metrics may share a packet when separated by `\n`. Connectionless on UDP (port 8125); connection-oriented on TCP, where each metric MUST be terminated by `\n`.

**Human URL:** [docs/metric_types.md](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)

#### Tags
- Counters, Gauges, Histograms, Line Protocol, Meters, Metrics, Sets, Timers, UDP, Wire Protocol

#### Properties
- [Documentation](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)
- [ProtocolSpec — b/statsd_spec](https://github.com/b/statsd_spec)
- [GitHubRepository](https://github.com/statsd/statsd)
- [AsyncAPI](asyncapi/statsd-wire-protocol-asyncapi.yml)
- [JSONSchema — Metric Line](json-schema/statsd-metric-line-schema.json)
- [JSONSchema — Metric Instance](json-schema/statsd-metric-instance-schema.json)
- [JSONStructure — Metric Instance](json-structure/statsd-metric-instance-structure.json)
- [JSON-LD Context](json-ld/statsd-context.jsonld)
- [Vocabulary](vocabulary/statsd-vocabulary.yml)

### StatsD Admin Interface
A plain-text TCP service on port 8126 (default). Supported commands: `stats`, `counters`, `gauges`, `timers`, `delcounters`, `delgauges`, `deltimers`, `health [up|down]`, `config`, `quit`. Wildcards (`*`) are supported on the delete commands. Intended for telnet/netcat-style operator use.

**Human URL:** [docs/admin_interface.md](https://github.com/statsd/statsd/blob/master/docs/admin_interface.md)

#### Tags
- Admin, Health Check, Management, Operator, TCP

#### Properties
- [Documentation](https://github.com/statsd/statsd/blob/master/docs/admin_interface.md)
- [OpenAPI](openapi/statsd-admin-interface-openapi.yml)
- [Naftiko Capability](capabilities/statsd-admin.yaml)
- [Spectral Ruleset](rules/statsd-admin-interface-rules.yml)
- [Example — stats command](examples/statsd-admin-stats-example.json)

### DogStatsD Wire Protocol
Datadog's StatsD-compatible ingestion protocol — a strict superset of vanilla StatsD that adds first-class tag syntax (`|#k:v,k:v`), histogram (`|h`) and distribution (`|d`) metric types, events (`_e{}`), service checks (`_sc|`), and Unix Domain Socket transport. Adopted by Stripe Veneur, Atlassian gostatsd, Telegraf, and Shopify statsd-instrument.

**Human URL:** [docs/metric_types.md](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)

#### Tags
- Datadog, Distributions, DogStatsD, Events, Histograms, Service Checks, Tags, Unix Domain Socket, Wire Protocol

#### Properties
- [Documentation](https://github.com/statsd/statsd/blob/master/docs/metric_types.md)
- [AsyncAPI](asyncapi/dogstatsd-wire-protocol-asyncapi.yml)
- [JSONSchema — Event](json-schema/dogstatsd-event-schema.json)
- [JSONSchema — Service Check](json-schema/dogstatsd-service-check-schema.json)
- [Example — Tagged Counter](examples/dogstatsd-tagged-counter-example.json)
- [Example — Distribution](examples/dogstatsd-distribution-example.json)
- [Example — Event](examples/dogstatsd-event-example.json)
- [Example — Service Check](examples/dogstatsd-service-check-example.json)

## Wire-Protocol Cheat Sheet

| Type | Suffix | Example | Aggregation |
|---|---|---|---|
| Counter | `\|c` | `gorets:1\|c` | Sum per flush; reset to zero |
| Counter (sampled) | `\|c\|@r` | `gorets:1\|c\|@0.1` | Sum scaled by `1/r` |
| Gauge | `\|g` | `gaugor:333\|g` | Last value, retained |
| Gauge (delta) | `\|g` w/ `+`/`-` | `gaugor:-10\|g` | Adjust retained value |
| Timer | `\|ms` | `glork:320\|ms` | count/sum/mean/lower/upper/percentiles |
| Histogram | `\|h` | `render:0.42\|h` | Per-bin counts |
| Distribution | `\|d` | `request.latency:42\|d` | Global percentiles (DogStatsD) |
| Set | `\|s` | `uniques:765\|s` | Distinct cardinality |
| Meter | `\|m` | `pageviews:1\|m` | Per-second event rate |
| Tags (DogStatsD) | `\|#k:v` | `page.views:1\|c\|#env:prod` | Dimensional split |

## Daemon Defaults

| Setting | Default | Notes |
|---|---|---|
| `port` (UDP) | 8125 | Metric ingest |
| `mgmt_port` (TCP) | 8126 | Admin interface |
| `address` | 0.0.0.0 | Bind address |
| `flushInterval` | 10000 ms | Backend flush cadence |
| `percentThreshold` | `[90]` | Timer percentiles |
| `deleteIdleStats` | false | Emit zeros for inactive buckets |
| `prefixStats` | `statsd` | Self-stat prefix |
| `healthStatus` | `up` | Initial admin health |

Source: `exampleConfig.js` in the upstream repo.

## Ecosystem

### Compatible Servers
[DogStatsD (Datadog Agent)](https://docs.datadoghq.com/developers/dogstatsd/) · [Telegraf StatsD plugin](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/statsd) · [Stripe Veneur](https://github.com/stripe/veneur) (Go; archived 2025-04) · [Atlassian gostatsd](https://github.com/atlassian/gostatsd) (Go) · [GitHub brubeck](https://github.com/github/brubeck) (C) · [statsite](https://github.com/armon/statsite) (C) · [statsd-c](https://github.com/jbuchbinder/statsd-c) (C) · [Wayfair statsdcc](https://github.com/wayfair/statsdcc) (C++) · [Avito bioyino](https://github.com/avito-tech/bioyino) (Rust) · [bitly statsdaemon](https://github.com/bitly/statsdaemon) · [vimeo statsdaemon](https://github.com/vimeo/statsdaemon) · [netdata](https://github.com/netdata/netdata)

### Bundled Backends
`graphite` (default), `console`, `repeater`. Plus 30+ third-party backends including AWS CloudWatch, InfluxDB, Datadog, OpenTSDB, Stackdriver, Elasticsearch, AMQP, Coralogix, Zabbix, Ganglia, MySQL.

### Client Libraries (selected)
- **Node.js**: lynx, node-statsd, node-statsd-client, statsy
- **Python**: pystatsd, Py-Statsd, python-statsd, Django-Statsd, datadogpy
- **Ruby**: Shopify statsd-instrument, reinh/statsd
- **Java**: java-statsd-client, statsd-netty, play-statsd
- **Go**: go-statsd, go-statsd-client, quipo/statsd
- **PHP**: thephpleague/statsd, statsd-php, php-statsd-client
- **.NET**: JustEat.StatsD, NStatsD.Client, graphite-client
- **Perl**: Net::Statsd, Etsy::StatsD, Metrics::Any::Adapter::Statsd

Full list on the upstream [wiki](https://github.com/statsd/statsd/wiki).

## Repository Layout

```
statsd/
├── apis.yml                              # APIs.json provider index
├── README.md
├── openapi/
│   └── statsd-admin-interface-openapi.yml
├── asyncapi/
│   ├── statsd-wire-protocol-asyncapi.yml
│   └── dogstatsd-wire-protocol-asyncapi.yml
├── json-schema/
│   ├── statsd-metric-line-schema.json
│   ├── statsd-metric-instance-schema.json
│   ├── dogstatsd-event-schema.json
│   └── dogstatsd-service-check-schema.json
├── json-structure/
│   └── statsd-metric-instance-structure.json
├── json-ld/
│   └── statsd-context.jsonld
├── examples/                             # 13 wire-format & admin examples
├── capabilities/
│   └── statsd-admin.yaml                 # Naftiko capability
├── rules/
│   └── statsd-admin-interface-rules.yml  # Spectral ruleset
└── vocabulary/
    └── statsd-vocabulary.yml
```

## Common Properties

- [GitHub](https://github.com/statsd/statsd)
- [License (MIT)](https://github.com/statsd/statsd/blob/master/LICENSE)
- [Change Log](https://github.com/statsd/statsd/blob/master/CHANGELOG.md)
- [Contributor Guide](https://github.com/statsd/statsd/blob/master/CONTRIBUTING.md)
- [Wiki](https://github.com/statsd/statsd/wiki)
- [Community Protocol Spec — b/statsd_spec](https://github.com/b/statsd_spec)
- [NPM Package](https://www.npmjs.com/package/statsd)
- [Issues](https://github.com/statsd/statsd/issues)
