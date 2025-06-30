# Step 3: Connect a Cluster

### Checklist

* [ ] The agent has been successfully deployed, and all pods are running in a healthy state.
* [ ] The user has been successfully created with all required permissions.

### Step 1: Connect a new cluster

<div align="left"><figure><img src="../.gitbook/assets/screenshot-with-background (1).png" alt=""><figcaption></figcaption></figure></div>

### Step 2: Fill in cluster details

Each vendor has a slightly different connection approach

#### Confluent Cloud / AWS MSK / Aiven

Automatic cluster discovery will initiate once an API key is provided.\
Metrics will be collected via the vendor API.

#### Redpanda / Apache (Self-hosted)

No automatic cluster discovery. Each cluster should be added manually. \
To enable metric collection in Superstream, a JMX connection must also be configured.

### Step 3: Verify that all discovered or added clusters are in a healthy state

When clusters are added or discovered, the system may surface warnings related to permissions or network connectivity. It’s recommended to resolve these promptly to ensure proper functionality.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% content-ref url="step-4-whats-next.md" %}
[step-4-whats-next.md](step-4-whats-next.md)
{% endcontent-ref %}
