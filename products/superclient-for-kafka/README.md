# SuperClient for Kafka

## Auto-Tune Your Kafka Clients For Efficiency & Governance

Superclient for Kafka eliminates guesswork by tuning each client’s config to its unique workload—driving performance gains, cost savings, and reduced network usage.

![Icon](https://cdn.prod.website-files.com/6834bf545aa0a8461c3bc7e7/6834bf545aa0a8461c3bcad0_Vector%20\(52\).svg) No Code Changes

There is no way we will ask someone to change their code! You just need to add a package or instrument your app.

![Icon](https://cdn.prod.website-files.com/6834bf545aa0a8461c3bc7e7/6834bf545aa0a8461c3bcad0_Vector%20\(52\).svg) Deep Observability

Perfect enhancement for easier, faster issue identification and resolution.

![Icon](https://cdn.prod.website-files.com/6834bf545aa0a8461c3bc7e7/6834bf545aa0a8461c3bcad0_Vector%20\(52\).svg) Dynamic Configuration

No more static, one-size-fits-all configuration. Superclient will dynamically auto-tune your client's config to match their current workload.

![Icon](https://cdn.prod.website-files.com/6834bf545aa0a8461c3bc7e7/6834bf545aa0a8461c3bcad0_Vector%20\(52\).svg) Client Governance

Ensure your clients run with your defined policies and best practices.

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

{% content-ref url="getting-started.md" %}
[getting-started.md](getting-started.md)
{% endcontent-ref %}
