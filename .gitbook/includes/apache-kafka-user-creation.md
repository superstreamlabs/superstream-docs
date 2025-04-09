---
title: Apache Kafka User Creation
---

### Step 1: Create a dedicated Kafka user for Superstream

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
  * `Delete`

ACL statement examples:

<pre><code><strong># Cluster-level permissions
</strong>kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Describe --cluster
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation DescribeConfigs --cluster
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Describe --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation List --topic '*'

# Topic-level permissions
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Read --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Alter --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Delete --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Describe --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation AlterConfigs --topic '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation DescribeConfigs --topic '*'

# Consumer group-level permissions
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation Describe --group '*'
kafka-acls --authorizer-properties zookeeper.connect=&#x3C;ZK_HOST:PORT> --add --allow-principal User:&#x3C;USER> --operation List --group '*'
</code></pre>

### Step 2: Connection information per cluster

The following information will be required for each cluster:

* Bootstrap servers (Kafka URL)
* Authentication security protocol (No auth / SSL / SASL\_SSL)
  * SSL with validation "on" would require a `key.pem`,`cert.pem`, and `ca.pem`&#x20;
* JMX port and token
