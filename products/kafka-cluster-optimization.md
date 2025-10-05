# Kafka Cluster Optimization

Superstream is automatically analyzing, optimizing, and remediating inefficiencies in Apache Kafka clusters. It helps platform teams keep their infrastructure lean, reliable, and cost-effective—without the manual grind of tuning broker configs or hunting down stale topics.

#### 🔍 Daily Cluster Health Scans

Superstream runs a full diagnostic sweep of your cluster every 24 hours. It inspects topic configurations, broker metadata, partition distribution, resource usage, and consumer group activity to detect misconfigurations or signs of inefficiency.

* Scans run automatically—no setup needed
* Results appear in the dashboard as a daily report
* Highlights include config anomalies, unused topics, and skewed partitions

This gives your team visibility into drift and decay that typically go unnoticed until there's an outage.

#### 🔧 Auto-Remediation

When enabled, Superstream doesn't just detect issues—it fixes them. Our automation engine applies safe remediations based on best practices, usage patterns, and your cluster-specific thresholds.

* Fixes misaligned configs (e.g., retention.ms, cleanup.policy)
* Resolves replication factor issues or ISR shrinkage
* Normalizes partition count and distribution

All remediations are logged and reversible. You can choose to run in observability-only mode first, and turn on automation gradually.

#### 📦 Cluster Right-Sizing

Superstream evaluates broker resource usage (CPU, memory, disk, throughput) and matches it against your current infrastructure provisioning.

* Works with AWS MSK, and Aiven
* Recommends better instance types or plan tiers
* Flags over-provisioned and under-resourced setups
* automatically performs safe reconfiguration—including intelligent partition rebalancing—to align resources with actual workload demand

This helps you reduce cloud costs without compromising performance.

#### 🧹 Idle Topics & Consumer Groups Cleanup

Clusters often accumulate unused topics and consumer groups over time. Superstream identifies those and can clean them up automatically (with protection rules, if needed).

* Detects topics with zero traffic or unassigned partitions
* Identifies idle consumer groups that haven’t polled in weeks
* Supports manual review or automated deletion

You can protect critical topics with exclusion rules to avoid accidental cleanup.

#### ⚙️ Topic Configuration Policies

Superstream allows you to enforce organization-wide policies on how Kafka topics should be configured. This ensures consistency, prevents drift, and reduces risk across your entire environment.

* Define global rules for critical configs like `retention.ms`, `retention.bytes`, `min.insync.replicas`, `replication.factor` and more.
* Detects and auto-corrects drifted or non-compliant topic settings
* Supports environment- or team-specific policies using tags or naming conventions
* Automatically applies corrections or flags for manual approval depending on your automation settings

These policies help standardize Kafka usage across services and teams—whether you’re running dozens or thousands of topics.

