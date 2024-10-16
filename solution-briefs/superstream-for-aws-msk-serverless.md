# Superstream for AWS MSK Serverless

Superstream enhances the experience of AWS MSK Serverless users in numerous ways, including significant cost reduction, improved performance, increased reliability, and robust governance.&#x20;

By optimizing resource usage and streamlining operations, Superstream ensures that users save money and achieve smoother, more efficient, and more secure data management. This comprehensive support helps AWS MSK Serverless users maximize their investment and maintain a high standard of operational excellence.

### Reducing costs

#### Your bill structure

* Cluster-hours
* Partition-hours
* Storage (Per GB-month)
* Data in
* Data out

#### How Superstream Can Help Reduce Costs

1. **Identify and Reduce Inactive Topics and Partitions:** Superstream employs proprietary methodologies to continuously detect and minimize inactive topics and partitions.
2. **Identify and Reduce Inactive Connections:** Superstream constantly monitors and reduces inactive connections, optimizing resource usage.
3. **Identify and Reduce Inactive Consumer Groups:** Superstream continuously tracks and diminishes inactive consumer groups to ensure efficient system performance.
4. **Optimize Producers without Compression:** Superstream assesses producers not using compression, determines their capability to enable it, and seamlessly activates the most suitable compression algorithm.
5. **Optimize Non-Binary Payloads:** Superstream identifies producers writing non-binary payloads and activates payload reduction techniques, ensuring no disruption to the existing workflow.

The entire identification process is fully configurable to accommodate various business logic while continuously self-improving through reinforcement learning.&#x20;

Although certain environments may not support all actions, implementing even a small portion can significantly reduce the monthly bill.

### Improve performance

#### Where are the bottlenecks in AWS MSK Kafka?

* Data Transfer: Limited network bandwidth can restrict the amount of data that can be transferred, especially in high-throughput scenarios.
* Producer Latency: High producer latency can occur due to batching, request sizes, or insufficient resources.
* Consumer Lag: Consumer lag occurs when consumers cannot keep up with the rate of messages produced.
* Large Number of Partitions: Managing a large number of partitions can strain broker resources and increase metadata overhead.
* Uneven Partition Distribution: Uneven partition distribution can lead to hotspots, where some brokers handle more load than others.
* Replication Lag: Replication lag occurs when follower replicas cannot keep up with the leader, leading to data inconsistencies.
* Acknowledgment Settings: Configuring `acks=all` can increase latency as the producer waits for acknowledgments from all replicas.

#### Here is how Superstream mitigates these issues

1. Ensure compression is enabled and enforce otherwise to reduce data size and optimize network usage.
2. Optimize producer configurations, such as batch size and linger.ms. Ensure producers have adequate CPU and memory resources.
3. Optimize consumer configurations, such as fetch size and max.poll.records. Scale consumer groups and partitions to distribute the load more effectively.
4. Optimize the number of partitions per topic based on throughput requirements.
5. Suggests improved key and partition strategies.
6. Ensure sufficient resources for follower replicas. Optimize replication settings, such as `min.insync.replicas` and `replication.factor`.
7. Adjust the ack parameter as needed.

### increase reliability

Achieving a high standard of reliability in Kafka involves three key components:

1. **Proactive Measures:** Enforce strict governance over clients to prevent abusive usage and apply necessary limitations to ensure optimal performance.
2. **Reactive Measures:** Acknowledge that "drifts" will occur and implement an additional layer of protection to address issues as they arise.
3. **High-availability** in case of a failure.

#### Here is how Superstream can help

1. **Proactive Measures:** Enforce client and cluster policies based on business groups, tags, or teams that determine what can and cannot be done across the different layers in terms of both configuration and client parameters.
2. **Reactive Measures:** If a client or user is still able to commit a violation, Superstream will identify it in its continuous scanning and mitigate it automatically or manually.
3.  **High-availability:** While you can use MirrorMaker 2.0, the solution is not hermetic. Here is why:

    1. You would still need to fix offset translations
    2. Clients will need to reroute to the DR cluster
    3. You would need to flush the clients' cache or build an internal mechanism to sync the leftover offsets directly in the DR cluster

    With the current version of MirrorMaker 2.0, Superstream complements it by addressing the challenges of failover situations with an extreme focus on clients.
