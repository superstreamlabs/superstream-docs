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

### Aiven

**Step 1: Create a Token**

1. In Aiven console: Click on user information (top right) -> Tokens -> Generate token
2. Use the created credentials in the Superstream console.

**Step 2: Creating a Kafka User**

1. Make sure the Kafka user you are giving to Superstream has the ACLs appear below.

### Other

Create a dedicated Kafka user for Superstream with the following ACLs

{% include "../.gitbook/includes/apache-kafka-user-creation.md" %}
