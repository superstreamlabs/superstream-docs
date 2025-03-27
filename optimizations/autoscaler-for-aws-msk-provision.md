# Autoscaler for AWS MSK Provision

uperstream achieves cost efficiency, enhanced performance, and greater reliability through continuous optimization of both application and infrastructure layers.

* Application layer: Identify and eliminate inactive resources, reduce traffic, apply improved configurations, enforce standardization, identify storage reduction opportunities, and more.
* Infrastructure layer: Clusters are often sized for peak times, but Superstream dynamically identifies the exact resources needed at any given moment and automatically scales accordingly.

### Reducing costs in AWS MSK

#### Your bill structure

AWS MSK Total Cost of Ownership (=TCO) includes:

* **Compute (Number of instances X type)**
* **Egress (Read) in GB/hour**
* **Storage in GB/hour**

#### Step 1: Optimizing the application layer (clusters/clients)

1. **Identify and Reduce Inactive Topics and Partitions:** Superstream employs proprietary methodologies to continuously detect and minimize inactive topics and partitions. Decreasing the number of partitions has a direct effect on Memory utilization and the number of needed brokers (instances)
2. **Identify and Reduce Inactive Connections:** Superstream constantly monitors and reduces inactive connections, optimizing resource usage.
3. **Identify and Reduce Inactive Consumer Groups:** Superstream continuously tracks and diminishes inactive consumer groups to ensure efficient system performance.
4. **Optimize Producers without Compression:** Superstream assesses producers not using compression, determines their capability to enable it, and seamlessly activates the most suitable compression algorithm.
5. **Optimize Non-Binary Payloads:** Superstream identifies producers writing non-binary payloads and activates payload reduction techniques, ensuring no disruption to the existing workflow.

The entire identification process is fully configurable to accommodate various business logic while continuously self-improving through reinforcement learning.&#x20;

#### Step 2: Dynamically scale the cluster's resources

Superstream leverages advanced machine learning algorithms to anticipate resource demands based on historical data and current usage patterns. By continuously analyzing this information, Superstream adjusts cluster sizes in real-time, ensuring that resources are allocated precisely when and where they are needed. This dynamic scaling guarantees optimal performance and significantly reduces operational costs by avoiding the need to provision resources for peak load conditions that may only occur sporadically.

Although certain environments may not support all actions, implementing even a small portion can significantly reduce the monthly bill.

### Getting started

#### Step 1: Access your MSK cluster through the Superstream Console

<figure><img src="../.gitbook/assets/Screenshot 2024-12-16 at 13.07.36.png" alt=""><figcaption></figcaption></figure>

#### Step 2: Scroll down to "Autoscaler" and define its rules

<figure><img src="../.gitbook/assets/Screenshot 2024-12-16 at 13.08.54.png" alt=""><figcaption></figcaption></figure>

#### Step 3: Activate

Superstream will now initiate and monitor the cluster, evaluating whether any defined rules are satisfied. If a rule is met, Superstream will execute the corresponding user-defined action.

***

### Ref: Scale-in a cluster (reduce the number of brokers)

{% embed url="https://www.loom.com/share/711010a49b9c4bf596e357a688bac422?sid=d3223723-8d5f-46ee-9fef-854f9047df01" %}

### Ref: How to manually scale an MSK cluster in case Superstream is down

### Overview

This manual provides step-by-step instructions for scaling Amazon Managed Streaming for Apache Kafka (MSK) clusters, including both horizontal and vertical scaling approaches.

### Horizontal Scaling (Adding Brokers)

#### Planning Phase

1. Monitor current cluster metrics:
   * CPU utilization
   * Storage utilization
   * Network throughput
   * Partition distribution
2. Calculate required capacity:
   * Number of partitions per broker
   * Expected throughput per broker

#### Implementation Steps

**Using AWS Console**

1. Navigate to the Amazon MSK console
2. Select your cluster
3. Click "Actions" → "Edit cluster configuration"
4. Under "Brokers", modify the number of brokers per Availability Zone
5. Review and confirm changes
6. Monitor the scaling operation in the console

**Using AWS CLI**

```bash
aws kafka update-broker-count \
    --cluster-arn <your-cluster-arn> \
    --current-version <current-cluster-version> \
    --target-number-of-broker-nodes <new-broker-count>
```

### Vertical Scaling (Broker Type Update)

#### Planning Phase

1. Identify target broker type based on:
   * CPU requirements
   * Memory needs
   * Network capacity requirements
   * Cost considerations

#### Implementation Steps

**Using AWS Console**

1. Navigate to the Amazon MSK console
2. Select your cluster
3. Click "Actions" → "Update broker type"
4. Select new broker type
5. Schedule the update
6. Review and confirm changes

**Using AWS CLI**

```bash
aws kafka update-broker-type \
    --cluster-arn <your-cluster-arn> \
    --current-version <current-cluster-version> \
    --target-instance-type <new-instance-type>
```

### Best Practices

* Scale during low-traffic periods
* Maintain sufficient headroom (20-30%) for unexpected traffic spikes
* Monitor scaling operations closely
* Keep cluster configuration version updated
* Document all scaling operations

### Troubleshooting

#### Common Issues

1. Insufficient Capacity Errors
   * Solution: Verify available capacity in target AZs
   * Contact AWS support if needed
2. Scaling Operation Timeout
   * Solution: Check AWS CloudWatch logs
   * Verify network connectivity
   * Review security group configurations
3. Uneven Partition Distribution
   * Solution: Run kafka-reassign-partitions tool
   * Review partition assignment strategy

### Monitoring and Maintenance

#### Key Metrics to Monitor

* Broker CPU utilization
* Storage utilization
* Network throughput
* Producer/consumer latency
* Partition replication lag

