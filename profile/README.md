## Security for the people who run infrastructure

Whoever runs the infrastructure holds the keys to everything on it: the
identity provider, the clusters, the firewall policy, the pager. Most of the
tools for that work treat security as a feature to add later. Here it is the
starting point.

**dragon-sec** is building a suite of tools for that work, one product at a
time. Each is its own service with its own releases, and they talk to each other
over open protocols — OpenID Connect for identity, OpenAPI for everything else —
not a shared database.

| Product | | What it is | Status |
| --- | --- | --- | --- |
| **ScaleLock** | *Guard the keys to your kingdom.* | Multi-tenant identity provider — OpenID Connect, single sign-on, passkeys and TOTP, roles and groups, audit log | In construction |
| **DragonFlight** | *Command your clusters with precision.* | Kubernetes management — clusters, access from ScaleLock, GitOps deploys, secrets sync | Planned |
| **DragonHoard** | *Guard your digital treasure.* | Data centre and IP address management — devices, address space, assets and owners | Planned |
| **DragonFire** | *Automate your defenses with fire and scale.* | Firewall automation across vendors — policy diffs, approvals, audit | Planned |
| **DragonWatch** | *When danger stirs, we wake first.* | Incident response and on-call — paging, routing, schedules, postmortems | Planned |
| **InferSight** | *See through the smoke.* | Observability — OpenTelemetry ingestion, dashboards, anomaly alerts | Planned |
| **ScaleForge** | *Forge infrastructure that endures.* | Infrastructure-as-code management — templates, drift detection, policy as code | Planned |
| **FlameRunner** | *Ship at the speed of fire.* | Delivery pipelines — pipelines as code, deployment gates, SBOM and provenance checks | Planned |
| **FireSight** | *Illuminate threats before they strike.* | Threat intelligence — enrichment, indicators, detection rules | Planned |
| **OathScale** | *Integrity forged in fire.* | Compliance and audit — evidence collection, control mapping, tamper-evident logs | Planned |

ScaleLock comes first because everything else signs in through it.

**Status: early.** Nothing is released yet.
