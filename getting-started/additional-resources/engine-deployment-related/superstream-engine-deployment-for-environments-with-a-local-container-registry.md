---
description: >-
  How to deploy a Superstream Agent in a fully air-gapped environment with a
  private container registry.
---

# Superstream Agent deployment for environments with a local container registry

This guide focuses on critical applications managed by Superstream Helm Chart: Telegraf, and Superstream. We will cover each application's specific Docker images that are managed by the helm chart, ensuring you have the information to deploy Superstream in your environment.

## Prerequisites

### 1. Container images

Please store the following images in your container registry:

**Telegraf**: As a versatile agent for collecting, processing, and writing metrics, Telegraf is pivotal in monitoring and observability.

* Helm version: 1.8.57
* Container:
  * [docker.io/library/telegraf:1.34-alpine](http://docker.io/library/telegraf:1.34-alpine)

**Superstream**: The agent itself.

* Helm version: [Releases](https://github.com/superstreamlabs/helm-charts/releases/latest)
* Helm Chart URL: [https://superstream-agent.k8s.superstream.ai/](https://superstream-agent.k8s.superstream.ai/)
*   Containers:&#x20;

    * superstreamlabs/superstream-data-plane-be:latest
    * superstreamlabs/superstream-kafka-auto-scaler:latest



To ensure that your private repositories use the correct Docker images, follow these steps to pull images from public repositories and tag them for your private repository. Below are command examples for the related Docker images you might use:

Telegraf (docker.io/library/telegraf:1.34-alpine):

```bash
docker pull library/telegraf:1.34-alpine
docker tag library/telegraf:1.34-alpine YOURREPOSITORY/library/telegraf:1.34-alpine
docker push YOURREPOSITORY/library/telegraf:1.34-alpine
```

Superstream Agent (superstreamlabs/superstream-data-plane-be:latest):

```bash
docker pull superstreamlabs/superstream-data-plane-be:latest
docker tag superstreamlabs/superstream-data-plane-be:latest YOURREPOSITORY/superstreamlabs/superstream-data-plane-be:latest
docker push YOURREPOSITORY/superstreamlabs/superstream-data-plane-be:latest
```

Superstream Autoscaller (superstreamlabs/superstream-kafka-auto-scaler:latest):

```bash
docker pull superstreamlabs/superstream-kafka-auto-scaler:latest
docker tag superstreamlabs/superstream-kafka-auto-scaler:latest YOURREPOSITORY/superstreamlabs/superstream-kafka-auto-scaler:latest
docker push YOURREPOSITORY/superstreamlabs/superstream-kafka-auto-scaler:latest
```



## Getting started

### 1. Download Helm Chart

Download the Superstream Helm chart from the official source as described above.&#x20;

### 2. Publish to Private Environments

Once downloaded, publish the chart to your private Helm chart repositories. This step ensures that you maintain control over the versions and configurations of the chart used in your deployments.&#x20;

Docker Image Names: You must change the Docker image names within the Helmfile to reflect those stored in your private Docker registries. This customization is crucial for ensuring that your deployments reference the correct resources within your secure environment:

### 3. Configure All Services to Use a Private Docker Repository and Private PullSecret

For easiness create/use `custom_values.yaml` file and add `global.image` section values, an example can be found [here](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/custom_values.yaml):

````yaml
```yaml
############################################################
# GLOBAL configuration for Superstream Agent
############################################################
global:
  agentName: ""                       # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""            # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""      # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  
  image:
    # global image pull policy to use for all container images in the chart
    # can be overridden by individual image pullPolicy
    pullPolicy:
    # global list of secret names to use as image pull secrets for all pod specs in the chart
    # secrets must exist in the same namespace
    # https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    pullSecretNames: []
    # global registry to use for all container images in the chart
    # can be overridden by individual image registry
    registry:
```
````

### 4. Deploy

To apply the Helmfile configurations and deploy your Kubernetes resources:

Apply Helmfile: Run the following command to apply the Helmfile configuration. This will sync your Helm releases to match the state declared in your `helmfile.yaml`:

{% code overflow="wrap" %}
```bash
helm repo add superstream <YOURREPOSITORY> --force-update
helm install superstream superstream/superstream -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}
