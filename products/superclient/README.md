# SuperClient for Kafka

### Intelligent Kafka Client Optimization

SuperClient is Superstream’s solution for tuning and monitoring your Kafka clients—automatically and at scale. It analyzes real-time producer behavior, recommends or applies optimized client configurations, and helps platform teams reduce data transfer costs, improve throughput efficiency, and minimize load on Kafka brokers. Whether you’re running hundreds of microservices or a handful of batch jobs, SuperClient ensures your clients are well-behaved, efficient, and production-ready.

#### 📡 Real-Time Client Observability

SuperClient continuously tracks Kafka producer activity and surfaces insights into how each client interacts with the system.

* Monitors throughput, compression ratios, batching, and message sizes
* Tracks client metadata like environment, and topic usage
* Highlights inefficient producers or topics

This observability allows teams to understand behavior patterns that directly affect broker load, latency, and throughput.

#### 🧠 Smart Client Config Recommendations

SuperClient recommends optimal Kafka producer configurations based on observed patterns—without requiring any changes to your application code.

* Suggests values for `batch.size`, `linger.ms`, `compression.type`
* Recommendations tailored to actual runtime behavior and topic-level throughput
* Includes per-topic savings estimates and efficiency scores

By tuning these parameters, SuperClient helps reduce broker CPU utilization, shrink network overhead, and stabilize throughput at scale.

#### 📊 Topic-Level Savings Reports

Every optimization is tied to real, measurable impact. SuperClient provides detailed reporting on how much you’re saving—and where.

* Visualize total data transfer and compute usage per topic
* See estimated cost and resource savings after applying suggestions
* Identify which clients or topics are most impactful to optimize

This helps you prioritize tuning efforts and demonstrate the value of optimization.

#### 📉 Reduce Broker Load and Stabilize Infrastructure

Client misconfigurations—like sending too many small messages or not compressing data—put unnecessary pressure on Kafka brokers. SuperClient mitigates this at the source.

* Reduces broker-side CPU and memory load
* Helps avoid backpressure, ISR flapping, and queue buildup
* Leads to smoother consumer behavior and more predictable system throughput

Less noisy clients mean healthier Kafka clusters with fewer fire drills.

#### 🧩 Instrumentation-Only, No Code Changes Required

SuperClient integrates into your Kafka ecosystem as an instrumentation layer. It observes producer behavior and injects optimized configuration without requiring developers to modify application code.

* Fully decoupled from client code

This design enables organizations to enforce optimization standards and roll out tuning at scale—without introducing friction into developer workflows.

### How

1. Superstream's local agent is deployed in your VPC and securely connects to designated clusters.
2. Continuous analysis is performed per topic and partition, outputting the current recommended set of properties to maximize network efficiency.
3. From this point, there are two options to proceed:
   1. Manual changes – Operators or engineers can review the recommended properties and apply them manually for each producer and its source code / `client.properties`.
   2. Automatic changes – Use the SuperClient for Kafka library. This library acts as a sidecar—an interceptor between the Superstream control plane and individual applications.
      1. Each application, during initialization and when connecting to **an already analyzed** topic, will receive an optimized set of properties tailored to its workload and topics.
      2. SuperClient will overwrite any existing properties—such as `compression.type`, `batch.size`, and `linger.ms`—with optimized values.
      3. Results should be visible immediately through the SuperClient Console or any other third-party APM tool.

<figure><img src="../../.gitbook/assets/SuperClient sequence diagram.png" alt=""><figcaption></figcaption></figure>

### FAQ

**Q:** How dynamic are the config changes? I.e., How often are the optimizations re-evaluated and potentially changed?

**A:** There are two ongoing processes involved:

1. Daily Workload Analysis\
   Every day, the system performs a workload analysis that may identify more optimal configuration properties. This means new recommendations could, in theory, be available on a daily basis.
2. Application Restart Required for Changes\
   However, for any new properties to take effect, the application must be restarted. Once the application starts with a particular set of optimized properties, it will continue operating with those settings until the next manual restart, rebuild, or redeployment.

**Q:** We have some producers in Kafka clusters that we didn’t previously connect. Do we need to do anything for these clusters to work with SuperClient?

**A:** First, ensure the new cluster is connected, and the Superstream local agent has permission to analyze it. Then, install the Superclient package — and that’s it.

{% content-ref url="getting-started-java.md" %}
[getting-started-java.md](getting-started-java.md)
{% endcontent-ref %}
