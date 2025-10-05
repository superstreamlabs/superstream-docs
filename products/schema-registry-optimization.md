# Schema Registry Optimization

Superstream helps Confluent Cloud users reduce Schema Registry costs by automatically identifying and cleaning up unused schemas.\
This feature is built specifically for environments using Confluent’s Schema Registry with the **topic name strategy**, where schema proliferation can quickly inflate costs and clutter your registry.

#### 🧹 Unused Schema Cleanup

Over time, Kafka clusters often accumulate schemas tied to topics that no longer exist or have been renamed. These orphaned schemas continue to occupy storage and increase billing, even though they serve no active producers or consumers.

Superstream automatically detects and cleans up those unused schemas:

* Scans schema subjects and versions daily
* Identifies schemas with no matching topic in the environment (based on topic name strategy)
* Flags inactive or orphaned schemas for cleanup
* Optionally auto-deletes them according to your automation settings

You can choose to **run in observability-only mode** to preview cleanup recommendations, or enable **full automation** to let Superstream safely remove stale schemas on its own.\
All cleanup actions are logged and reversible.

#### 💰 Why It Matters

Confluent Cloud charges per stored schema and version. Over time, orphaned schemas can significantly inflate registry costs—especially in dynamic environments with frequent topic churn.\
By automatically cleaning them up, Superstream helps you:

* Reduce Confluent Schema Registry costs
* Keep your registry organized and lightweight
* Avoid manual cleanup or risk of deleting active schemas

Schema Registry Optimization is available **exclusively for Confluent Cloud** users and integrates seamlessly with Superstream’s cluster health scans and automation engine.

