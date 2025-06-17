---
cover: ../../.gitbook/assets/BYOC.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Step 1: Agent Deployment

Superstream "Bring Your Own Cloud" is the perfect solution for customers who prefer or can't have an external connection from outside their cloud to their Kafka clusters.

<figure><img src="../../.gitbook/assets/Superstream architecture.png" alt=""><figcaption></figcaption></figure>

### Step 1: Check out our "Environment Readiness Checklist"

Review the [following checklist](https://docs.google.com/spreadsheets/d/1z-IRt6jBhMpL-T9XhL0k1hoPHgAZnlSoPh0ay2ymses/edit?usp=sharing) to ensure your environment is ready for deployment and to avoid potential issues.

### Step 2: Deploy

Superstream is using a Helm chart to deploy its agent.

You can deploy as many agents as needed and spread your clusters between them based on your environmental preferences.

#### The Superstream local agent chart deploys the following pods:

* `superstream-data-plane` : Responsible for communicating with connected Kafka clusters, collecting required metadata, and processing it to surface insights that serve both SuperClient and SuperCluster.
* `superstream-auto-scaler` : Optional. Responsible for automatically scaling AWS MSK and Aiven Kafka clusters."
* `superstream-syslog` : Responsible for monitoring Superstream-deployed pods.

<figure><img src="../../.gitbook/assets/Superstream deployment arch (2).png" alt=""><figcaption></figcaption></figure>

#### 1. Configure the `custom_values.yaml` file

Create a `custom_values.yaml` file and edit the relevant values (An example can be found [here](https://github.com/superstreamlabs/helm-charts/blob/master/charts/superstream-agent/custom_values.yaml)).

You can always get your `superstreamAccountId` and `superstreamActivationToken` through the console or within the automatic email you received when you signed up.

{% code title="custom_values.yaml" lineNumbers="true" %}
```yaml
############################################################
# GLOBAL configuration for Superstream Agent
############################################################
global:
  agentName: ""                       # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""            # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""      # Enter the activation token required for services or resources that need an initial token for activation or authentication.
```
{% endcode %}

#### 2. Deploy

Head over to the `custom_values.yaml` file location and run:

{% code overflow="wrap" lineNumbers="true" %}
```bash
helm repo add superstream-agent https://superstream-agent.k8s.superstream.ai/ --force-update && helm upgrade --install superstream superstream-agent/superstream-agent -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}

Deployment verification:

{% code lineNumbers="true" %}
```bash
helm list
```
{% endcode %}

## Appendixes

### Appendix A - Superstream Update

1. Retrieve the Most Recent Version of the Superstream Helm Chart

```yaml
 helm repo add superstream-agent https://superstream-agent.k8s.superstream.ai/ --force-update
```

2. Make sure to use the same values:

```
helm get values superstream --namespace superstream
```

3. Run the Upgrade command:&#x20;

```yaml
helm upgrade --install superstream superstream-agent/superstream-agent -f custom_values.yaml --namespace superstream --wait
```

### Appendix B - Uninstall

**Steps to Uninstall Superstream Agent:**

1. Delete Superstream Agent Helm Releases:

```
helm delete superstream -n <NAMESPACE>
```

### Appendix C - Deploy Superstream Agent using labels, tolerations, nodeSelector and etc'

* To inject custom labels into all services deployed by Superstream, utilize the `global.labels` variable.&#x20;

```yaml
############################################################
# GLOBAL configuration for Superstream Agent
############################################################
global:
  agentName: ""                    # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  
  labels:
    tests: ok
```

* To configure tolerations, nodeSelector, and affinity settings for each deployed service, the adjustments in the following example need to be done:

```yaml
############################################################
# GLOBAL configuration for Superstream Agent
############################################################
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
```

### Appendix D - Deploy Superstream from local registry

* To deploy Superstream from local registry, override default values using `global.image` variable in  `custom.values`file:

```yaml
############################################################
# GLOBAL configuration for Superstream Agent
############################################################
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



## Best practices

### Dev / Staging environments

Connecting your Development/Staging Kafka Clusters to Superstream is recommended. This can be done using either one or more dedicated Superstream Agents (data planes) for each environment or the same Agent connected to the production clusters.
