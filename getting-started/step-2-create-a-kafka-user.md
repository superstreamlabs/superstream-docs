---
cover: ../.gitbook/assets/BYOC.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Step 2: Create a Kafka user

Superstream requires a Kafka user with the following configuration to communicate and analyze connected clusters.

## By Kafka flavor/vendor:

### AWS MSK

{% include "../.gitbook/includes/msk-user-creation.md" %}

***

### Confluent Cloud

{% include "../.gitbook/includes/confluent-cloud-user-creation.md" %}

### Apache Kafka (Self-hosted)

{% include "../.gitbook/includes/apache-kafka-user-creation.md" %}
