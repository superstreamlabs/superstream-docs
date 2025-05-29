---
description: This page describes the processed data and metadata by Superstream engine
---

# Processed data

The data stays on the customer's premises, and only the metadata, which consists of calculated results, is transmitted according to the customer's choice to either:

* The Superstream control plane located off the premises.&#x20;
* An on-premises control plane ensures that no data whatsoever leaves the premises.

### A specific list of data to be fetched from each Kafka cluster:

Topic Metadata:

* List of Topics: Retrieve all available topics in the Kafka cluster.
* Topic Configuration: Access detailed configuration settings for each topic, such as retention policies, partition count, replication factor, activity events, connected CGs, and segment file settings.
* Partition Information: Details about each topic's partitions, including partition IDs and their current leaders.
* Replicas placements
* Payload samples: The component that consumes the payload is the deployed local agent, which runs within your premises and, after analyzing the payload structure (regardless of its contents), immediately dumps it.

Consumer Group Metadata:

* Consumer Group List: List of all consumer groups connected to the Kafka cluster.
* Consumer Group Offsets: Information about the offset position for each consumer group in each partition of a topic.
* Consumer Group State: Current state of consumer groups, like whether they are active or in a rebalance phase.
* Static membership

Broker Metadata:

* Broker IDs and Addresses: Information about each broker in the Kafka cluster, including their IDs and network addresses.
* Broker Configurations: Configuration details of each broker, like log file size limits, message size limits, and more.
* Broker Metrics and Health: Data about broker performance, such as CPU and memory usage, network I/O, and throughput metrics.
* Rack aware id

Cluster Metadata:

* Cluster ID: Unique identifier of the Kafka cluster.
* Controller Broker Info: Details about the current controller broker, which is responsible for maintaining the leader election for partitions.

Log Metadata:

* Log Size and Health: Information on the log size for each topic and partition, and details on log segment files.
