# Superstream for Confluent Cloud

Superstream enhances the experience of Confluent Cloud users in numerous ways, including significant cost reduction, improved performance, increased reliability, and robust governance.&#x20;

By optimizing resource usage and streamlining operations, Superstream ensures that users not only save money but also achieve smoother, more efficient, and more secure data management. This comprehensive support helps Confluent Cloud users maximize their investment and maintain a high standard of operational excellence.

### Reducing costs

#### Your bill structure

* **E-CKU**: Represents the maximum number of partitions and connections an account can reach.
* **Ingress (Write) in GB/hour**
* **Egress (Read) in GB/hour**
* **Storage in GB/hour**

Whether you are using a dedicated or an enterprise cluster, cost reduction is not measured by decreasing your current spending only but also by making sure you are not increasing it.

#### How Superstream Can Help Reduce Costs

1. **Identify and Reduce Inactive Topics and Partitions:** Superstream employs proprietary methodologies to continuously detect and minimize inactive topics and partitions.
2. **Identify and Reduce Inactive Connections:** Superstream constantly monitors and reduces inactive client connections, optimizing connection usage.
3. **Identify and Reduce Inactive Consumer Groups:** Superstream continuously tracks and diminishes inactive consumer groups to ensure efficient system performance.
4. **Reduce unused schemas**
5. **Transfer reduction:** Superstream will automatically identify the most optimal compression and batch.size per each type of workload/topic
6. **Optimize Non-Binary Payloads:** Superstream identifies producers writing non-binary payloads and activates payload reduction techniques, ensuring no disruption to the existing workflow.

The identification and remediation process is fully configurable to accommodate various business logic while continuously self-improving through reinforcement learning.&#x20;

Although certain environments may not support all actions, implementing even a small portion can significantly reduce the monthly bill.

### A Faster Catch and Respond to Issues

1. Anomaly Detection: Know Before it Breaks\
   Superstream’s AI-powered anomaly detection continuously monitors your Kafka clusters, automatically identifying irregular patterns in message rates, consumer lag, partition imbalance, and system bottlenecks. Instead of relying on static thresholds, our system adapts to your workload, highlighting potential failures before they cause downtime.
2. Troubleshoot with Insights, Not Charts\
   Traditional Kafka monitoring tools flood you with dashboards and metrics—but when something goes wrong, you need answers, not just data. Superstream translates complex Kafka behavior into clear, actionable insights. Instead of digging through charts and logs, you get direct recommendations on what's wrong, why it happened, and how to fix it. This means faster resolution times and fewer headaches for your team.
3. Kafka360: A Complete Picture of Your Streaming Infra\
   When troubleshooting, context is key. Kafka360 provides a holistic view of your Kafka ecosystem, bringing together producers, consumers, brokers, and performance metrics into a single, intuitive interface. This lets you trace issues across the entire data flow, pinpoint root causes, and optimize performance—all in real time.\
