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

### Confluent Cloud

For connecting Confluent Cloud clusters to Superstream, two types of API keys are required to be created:&#x20;

#### Step 1: Create a new Confluent service account

In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**

<figure><img src="../../.gitbook/assets/Screenshot 2025-01-14 at 10.02.26.png" alt=""><figcaption></figcaption></figure>

In the "Add service account" wizard:

1. **Name** the service account "`Superstream`"
2. Permissions ("+ Add role assignment"):
   1. For each **organization**: `BillingAdmin` and `MetricsViewer`
   2. For each **environment:** `MetricsViewer` ,`DataDiscovery`, `Operator`&#x20;
      1. For **environment** -> **Schema Registry**
         1. Select resource: `All schema subjects`&#x20;
         2. Select role: `ResourceOwner`&#x20;
   3. For each **cluster:** `CloudClusterAdmin` , `MetricsViewer`
      1. For each designated **cluster** -> **Topics**
         1. `DeveloperRead`: All topics
         2. `DeveloperManage`: All topics
      2. For each designated **cluster** -> **Consumer Groups**
         1. Read all `Consumer group`

#### Step 2: Create a Confluent Cloud Resource Management Key

In Confluent Console: Top-right menu -> API Keys -> + Add API key

Follow the following steps:

<div align="left"><figure><img src="../../.gitbook/assets/Screenshot 2024-10-06 at 20.37.52.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/Screenshot 2024-10-15 at 14.10.00.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/Screenshot 2024-10-06 at 20.39.28.png" alt="" width="375"><figcaption></figcaption></figure></div>

Create <mark style="color:red;">**and save the newly created credentials using the cluster name**</mark><mark style="color:red;">.</mark>

#### Step 3: Create a dedicated API key per cluster

In Confluent Console: Left menu -> Home -> Environments -> `<environment name>` -> `<cluster name>` -> API Keys

<div align="left"><figure><img src="../../.gitbook/assets/Screenshot 2024-10-15 at 13.42.03.png" alt="" width="172"><figcaption></figcaption></figure></div>

Click on "**+ Add key**"

<div align="left"><figure><img src="../../.gitbook/assets/Screenshot 2024-10-15 at 13.43.29.png" alt="" width="375"><figcaption></figcaption></figure></div>

* Choose "**Service account**" -> "`Superstream`" (The one we created in Step 1)
* ACLs:
  1. Cluster
     1. `ALTER_CONFIGS`: ALLOW
     2. `DESCRIBE`: ALLOW
     3. `DESCRIBE_CONFIGS`: ALLOW
  2. Consumer Group
     1. Rule 1:
        1. Consumer group ID: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `Delete`
        4. Permission: `ALLOW`
     2. Rule 2:
        1. Consumer group ID: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `Describe`
        4. Permission: `ALLOW`
     3. Rule 3:
        1. Consumer group ID: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `Read`
        4. Permission: `ALLOW`&#x20;
  3. Topic
     1. Rule 1:
        1. Topic name: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `ALTER`
        4. Permission: `ALLOW`
     2. Rule 2:
        1. Topic name: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `ALTER_CONFIGS`
        4. Permission: `ALLOW`
     3. Rule 3:
        1. Topic name: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `DELETE`
        4. Permission: `ALLOW`
     4. Rule 4:
        1. Topic name: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `DESCRIBE`
        4. Permission: `ALLOW`
     5. Rule 5:
        1. Topic name: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `DESCRIBE_CONFIGS`
        4. Permission: `ALLOW`
     6. Rule 6:
        1. Topic name: `superstream`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `Create`
        4. Permission: `ALLOW`
     7. Rule 7:
        1. Topic name: `*`&#x20;
        2. Pattern type: `LITERAL`&#x20;
        3. Operation: `READ`
        4. Permission: `ALLOW`
* Create <mark style="color:red;">**and save the newly created credentials using the cluster name**</mark><mark style="color:red;">.</mark>

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
