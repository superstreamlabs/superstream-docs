# Superstream for Redpanda

Superstream is an autonomous platform designed to optimize and improve Kafka of all types to reduce costs, reduce operations, and increase reliability.

Superstream enhances the experience of Redpanda Cloud users in numerous ways, including significant cost reduction and increased visibility.

By optimizing resource usage, Superstream ensures that Redpanda users not only save money but also achieve a more reliable streaming environment with deep observability and client-level insights.

### Reducing costs

#### Billed components

* **Uptime**
* **Ingress (Write)**
* **Egress (Read)**
* **Storage in GB/hour**
* **Partitions per hour**

#### How Superstream Can Help Reduce Costs

1. **Identify and Reduce Inactive Topics and Partitions:** Superstream employs proprietary methodologies to continuously detect and minimize inactive topics and partitions.
2. **Identify and Reduce Inactive Connections:** Superstream constantly monitors and reduces inactive connections, optimizing resource usage.
3. **Identify and Reduce Inactive Consumer Groups:** Superstream continuously tracks and diminishes inactive consumer groups to ensure efficient system performance.
4. **Utilize Producer-level Compression:** Superstream finds producers that are not using compression, determines their capability to enable it, its outcome, and surface who they are
5. **Identify your heavy requester:** Quickly identify how many requests each client performs and ways to reduce that.
6. **Optimize Non-Binary Payloads:** Superstream identifies producers writing non-binary payloads and activates payload reduction techniques, ensuring no disruption to the existing workflow.

The entire identification process is fully configurable to accommodate various business logic while continuously self-improving through reinforcement learning.&#x20;

Although certain environments may not support all actions, implementing even a small portion can significantly reduce the monthly bill.

### increase reliability

Achieving a high standard of reliability in Kafka involves three key components:

1. **Proactiveness:** Enforce strict configuration governance over clients and created resources to prevent abusive usage and apply necessary limitations to ensure optimal performance.
2. **Reactiveness:** Acknowledge that configuration "drifts" will occur and implement an additional layer of protection to address issues as they arise.
3. **Anomaly detection:** Identify issues and their potential root cause before they reach your customers and applications

**Superstream can help establish all three:**

1. **Proactive Measures:** Enforce client and cluster policies based on business groups, tags, or teams that determine what can and cannot be done across the different layers in terms of both configuration and client parameters.
2. **Reactive Measures:** If a client or user can still commit a violation, Superstream will identify it in its continuous scanning and quickly notify the operators.
3. **Anomaly detection:** Superstream will continuously learn your workloads and patterns, and alert when there is significant change in traditional patterns across clients and clusters.

<figure><img src="../.gitbook/assets/CTA (4).png" alt=""><figcaption></figcaption></figure>


[![Superstream](https://i.ibb.co/J9bjj0N/CTA.png)](https://app.superstream.ai/signup)
