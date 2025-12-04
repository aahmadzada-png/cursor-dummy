# Summary of KOF (k0rdent Observability & FinOps)

**Source:** https://docs.k0rdent.io/v1.5.0/admin/kof/

## What is KOF?

KOF (k0rdent Observability & FinOps) is a unified observability and FinOps platform designed for k0rdent Kubernetes estates. It bundles metrics, logs, traces, and cost tracking into one integrated system, eliminating the need to integrate and maintain multiple separate tools. It's specifically designed for multi-cluster environments (management + child clusters across regions/clouds).

## Key Problems It Solves

- **Avoids tool fragmentation**: No need to integrate and maintain multiple separate systems (Prometheus, Grafana, OpenCost, Jaeger, etc.)
- **Eliminates complexity**: Removes component selection/versioning complexity and CRD drift issues
- **Cross-cluster aggregation**: Provides estate-wide visibility without fragile federation setups
- **Standardization**: Ensures retention, access control, and network policies are consistent across the estate

## Core Components

- **Metrics**: VictoriaMetrics (vmcluster, vmauth)
- **Logs**: VictoriaLogs
- **Tracing**: Jaeger with OpenTelemetry
- **Cost**: OpenCost
- **Dashboards**: Grafana (managed by grafana-operator)
- **Aggregation**: Promxy for Prometheus-compatible fan-out
- **Control**: kof-operators for lifecycle management

## Architecture

- **Child clusters**: Always run collectors (OpenTelemetry, OpenCost) to gather data at source
- **Management cluster**: Runs Grafana, promxy, operators, and policy
- **Storage/Aggregation**: VictoriaMetrics, VictoriaLogs, and Jaeger can run in:
  - Management cluster (for management data)
  - Regional KOF deployment (collecting from child clusters in that region)
  - Third-party service for selected streams (e.g., logs exported to AWS CloudWatch)

## Key Benefits

- **Unified visibility**: Metrics, logs, traces, and costs all in one place
- **Actionable cost insights**: Workload spend visible alongside performance metrics
- **Standardized retention policies**: Documented options for data retention
- **Lifecycle management**: GitOps-based deployment and upgrades
- **Compliance and audit**: Central policy ensures retention, RBAC, and secure transport

## Deployment

- Deployed via Helm charts and MultiClusterServices
- Recommended to keep KOF configuration in a dedicated Git repository
- Managed through CI/CD pipelines for consistency
- Main charts include: kof-operators, kof-mothership, kof-storage, kof-collectors, plus per-role services such as kof-child and kof-regional

## Use Case Example

The documentation includes a scenario where:
1. **Finance** spots a 20% cost spike in the FinOps dashboard
2. **DevOps** traces it to a specific service via traces and metrics
3. **Platform Engineers** validate and fix it via GitOps
4. **Finance** confirms costs are back under control

All within a single day using unified KOF dashboards.

## DIY Stack vs. KOF Comparison

| Dimension | DIY Stack | KOF |
|-----------|-----------|-----|
| **Setup & Integration** | Assemble 4–6 separate projects, each with its own configs | One integrated platform: metrics, logs, traces, and costs shipped together |
| **Scale & Performance** | Prometheus federation slows at millions of samples | VictoriaMetrics/Logs designed for millions of samples/sec, fast queries |
| **Consistency** | Dashboards, alerts, and retention vary per cluster | GitOps-native: dashboards, alerts, and policies stored in Git |
| **Troubleshooting** | Metrics in one tool, logs in another, traces in a third | Unified Grafana dashboards: metrics, logs, and traces correlated automatically |
| **Cost Management** | OpenCost must be bolted on; engineers rarely look at it | Costs embedded in Grafana dashboards engineers already use |
| **Multi-Cluster Support** | Each cluster runs its own observability stack; federation is fragile | Single control plane for management, regional, and child clusters |
| **Compliance & Retention** | Long-term retention requires custom S3/Elasticsearch setups | Policy-driven retention (30–365+ days) and replication built in |
| **Security & Governance** | Role-based access is piecemeal, TLS often manual | RBAC and secure communication enforced by default |
| **Upgrades** | Upgrade each component independently; breakage risk is high | Guided version-to-version upgrades with clear migration paths |

## Getting Started

KOF lets you get started with quick wins:
- Spin up a single cluster and access Grafana dashboards in under 10 minutes
- Attribute cloud costs to teams immediately using OpenCost metrics
- Configure alerts for both performance and budget thresholds
- Start retaining logs and metrics for compliance right out of the box

## Extensibility

KOF is fully functional out of the box, but can be extended with:
- **Dashboards and alerts**: Add Grafana dashboards and VMRules in Git
- **Collector pipelines**: Extend OpenTelemetry collectors with custom receivers, processors, or exporters
- **External destinations**: Route specific streams to third-party systems (recipes included for CloudWatch and others)

All extensions are managed through the same GitOps lifecycle as the rest of the platform.

## Documentation References

- Architecture
- Installing KOF
- Verifying the KOF installation
- KCM Region With KOF
- Storing KOF data
- Using KOF
- KOF Alerts
- KOF Tracing
- Retention and Replication
- Resources Requirements
- Scaling KOF
- Maintaining KOF
- Upgrading KOF
- FAQ

For advanced guides, see: k0rdent/kof/docs

---

*Summary generated from: https://docs.k0rdent.io/v1.5.0/admin/kof/*
