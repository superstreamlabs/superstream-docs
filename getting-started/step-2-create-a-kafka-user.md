---
cover: ../.gitbook/assets/BYOC.jpg
coverY: 0
---

# Step 2: Create a Kafka User

Superstream requires a Kafka user with the following configuration to communicate and analyze connected clusters.

## By Kafka flavor/vendor:

### AWS MSK

{% include "../.gitbook/includes/msk-user-creation.md" %}

***

### Confluent Cloud

{% include "../.gitbook/includes/confluent-cloud-user-creation.md" %}

### Other (Apache Kafka (Self-hosted) / Aiven / Redpanda)

{% include "../.gitbook/includes/apache-kafka-user-creation.md" %}
