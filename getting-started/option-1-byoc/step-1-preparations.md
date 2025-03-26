---
cover: ../../.gitbook/assets/BYOC.jpg
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

Superstream requires a Kafka user with the below configuration to be able to communicate and analyze connected clusters.

## By Kafka flavor/vendor:

### AWS MSK

{% include "../../.gitbook/includes/msk-user-creation.md" %}

***

{% include "../../.gitbook/includes/confluent-cloud-user-creation.md" %}

### Apache Kafka (Self-hosted)

#### Step 1: Create a dedicated Kafka user for Superstream

For effective functioning, a user or token requires the following permissions:

* Cluster-level:
  * Describe all `topics`, List all topics, Describe configs, Describe cluster
* Topic-level:
  * `Read: All topics`
  * `Alter: All topics`
  * `Delete: All topics`
  * `Describe: All topics`
  * `Alter: All topics`
  * `AlterConfigs: All topics`
  * `DescribeConfigs: All topics`
* Consumer group-level:
  * `Describe`
  * `List Consumer Groups`

ACL statements examples:

{% code lineNumbers="true" %}
```bash
# Cluster-level permissions
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Describe --cluster
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation DescribeConfigs --cluster
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Describe --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation List --topic '*'

# Topic-level permissions
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Read --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Alter --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Delete --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Describe --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation AlterConfigs --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation DescribeConfigs --topic '*'

# Consumer group-level permissions
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation Describe --group '*'
kafka-acls --authorizer-properties zookeeper.connect=<ZK_HOST:PORT> --add --allow-principal User:<USER> --operation List --group '*'
```
{% endcode %}

#### Step 2: Connection information per cluster

The following information will be required for each cluster:

* Bootstrap servers (Kafka URL)
* Authentication security protocol (No auth / SSL / SASL\_SSL)
  * SSL with validation "on" would require a `key.pem`,`cert.pem`, and `ca.pem`&#x20;
* JMX port and token

<figure><img src="../../.gitbook/assets/Screenshot 2025-02-14 at 12.54.54.png" alt="" width="375"><figcaption></figcaption></figure>
