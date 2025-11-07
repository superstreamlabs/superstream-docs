---
cover: ../.gitbook/assets/BYOC.jpg
coverY: 0
---

# Step 1: Agent Deployment

Superstream BYOC lets you run agents inside your own cloud—ideal when Kafka clusters can’t be exposed externally.

* Deploy one or more agents, distributing clusters however you prefer.
* Ensure your Docker or Kubernetes environment has network access to the target Kafka clusters.
* Once the agent is running, you're ready for the next step.

<figure><img src="../.gitbook/assets/684688b1346415bfcb95b278_superstream architecture.svg" alt=""><figcaption></figcaption></figure>

### 🔑 Retrieve required info

In the Superstream [console](https://app.superstream.ai/) (under your user profile):

* **Account ID** – copy from console
* **Activation Token** – copy from console
* **Agent Name** – choose a unique name (max 32 chars, lowercase only).\
  Allowed: `a-z`, `0-9`, `-`, `_`\
  Not allowed: `.`

<figure><img src="../.gitbook/assets/Screenshot 2025-08-05 at 10.19.12.png" alt=""><figcaption></figcaption></figure>

### 🐳 Deploy via Docker

Run the following command to download and start the Superstream agent via Docker Compose:

```
ACCOUNT_ID=<account id> AGENT_NAME=<name> ACTIVATION_TOKEN=<token> bash -c 'curl -o docker-compose.yaml https://raw.githubusercontent.com/superstreamlabs/helm-charts/master/docker/docker-compose.yaml && docker compose up -d'
```

### ☸️ Deploy via Kubernetes

Superstream provides a Helm chart to deploy the agent.

1\. Create and configure `custom_values.yaml` . Define required values like account ID, activation token, and agent name → View [example](https://github.com/superstreamlabs/helm-charts/blob/master/charts/superstream-agent/custom_values.yaml).

{% code title="custom_values.yaml" %}
```yaml
global:
  agentName: ""               # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""
  superstreamActivationToken: ""
```
{% endcode %}

2\. Navigate to the directory containing your `custom_values.yaml` file and run:

{% code overflow="wrap" %}
```bash
helm repo add superstream-agent https://superstream-agent.k8s.superstream.ai/ --force-update && helm upgrade --install superstream superstream-agent/superstream-agent -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}

Deployment verification:

```bash
helm list
```

### 📦 What Gets Deployed

Whether using Docker or Kubernetes, the Superstream agent setup includes the following components:

* **`superstream-data-plane`**\
  Core service that connects to your Kafka clusters, collects metadata, and generates insights.
* **`superstream-auto-scaler`** _(optional)_\
  Automatically scales **AWS MSK** and **Aiven Kafka** clusters when enabled.
* **`superstream-telegraf`**\
  Monitors internal agent components for health and metrics.
* **`superstream-datadog`** _(optional)_\
  Collects and exports Kafka JMX metrics to Datadog, providing deep visibility into broker, topic, and consumer performance.

## Appendixes

### Appendix A - Superstream Update

1. Retrieve the most recent version of the Superstream Helm chart

```yaml
 helm repo add superstream-agent https://superstream-agent.k8s.superstream.ai/ --force-update
```

2. Make sure to use the same values:

```
helm get values superstream --namespace superstream
```

3. Run the upgrade command:&#x20;

```yaml
helm upgrade --install superstream superstream-agent/superstream-agent -f custom_values.yaml --namespace superstream --wait
```

### Appendix B - Uninstall

```
helm delete superstream -n <NAMESPACE>
```

### Appendix C - Deploy Superstream Agent using labels, tolerations, nodeSelector and etc'

* To inject custom labels into all services deployed by Superstream, utilize the `global.labels` variable.&#x20;

```yaml
global:
  agentName: ""                    # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  
  labels:
    tests: ok
```

* To configure tolerations, nodeSelector, and affinity settings for each deployed service, the adjustments in the following example need to be done:

```yaml
global:
  agentName: ""                    # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  
superstreamAgent:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"
autoScaler:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"
telegraf:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"
datadog:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"    
```

### Appendix D - Deploy Superstream from local registry

* To deploy Superstream from local registry, override default values using `global.image` variable in  `custom.values`file:

```yaml
global:
  agentName: ""                    # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  
  image:
    # Global image pull policy to use for all container images in the chart
    # can be overridden by individual image pullPolicy
    pullPolicy:
    # Global list of secret names to use as image pull secrets for all pod specs in the chart
    # secrets must exist in the same namespace
    # https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    pullSecretNames: []
    # Global registry to use for all container images in the chart
    # can be overridden by individual image registry
    registry: 
```
