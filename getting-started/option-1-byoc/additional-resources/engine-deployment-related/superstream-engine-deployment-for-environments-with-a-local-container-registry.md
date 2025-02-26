---
description: >-
  How to deploy a Superstream Engine in a fully air-gapped environment with a
  private container registry.
---

# Superstream Engine deployment for environments with a local container registry

This guide focuses on three critical applications managed by Superstream Helm Chart: NATS, Telegraf, and Superstream. We will cover each application's specific Docker images that are managed by the helm chart, ensuring you have the information to deploy Superstream in your environment.

## Prerequisites

### 1. Container images

Please store the following images in your container registry:

**NATS**: An open-source messaging system renowned for its high performance and scalability.

* Helm version: 1.2.4
* &#x20;Containers:
  * natsio/prometheus-nats-exporter:0.15.0
  * natsio/nats-server-config-reloader:0.15.1
  * Nats:2.10.20-alpine

**Telegraf**: As a versatile agent for collecting, processing, and writing metrics, Telegraf is pivotal in monitoring and observability.

* Helm version: 1.8.48
* Container:
  * [docker.io/library/telegraf:1.30-alpine](http://docker.io/library/telegraf:1.30-alpine)

**Superstream**: The engine itself.

* Helm version: [Releases](https://github.com/superstreamlabs/superstream-engine/releases/latest)
* Helm Chart URL: https://k8s.superstream.ai/
*   Containers:&#x20;

    * superstreamlabs/superstream-data-plane-be:latest
    * curlimages/curl:8.6.0
    * linuxserver/syslog-ng:4.5.0



To ensure that your private repositories use the correct Docker images, follow these steps to pull images from public repositories and tag them for your private repository. Below are command examples for the related Docker images you might use:

Prometheus NATS Exporter (natsio/prometheus-nats-exporter:0.15.0):

```bash
docker pull natsio/prometheus-nats-exporter:0.15.0 
docker tag natsio/prometheus-nats-exporter:0.15.0 YOURREPOSITORY/natsio/prometheus-nats-exporter:0.15.0 
docker push YOURREPOSITORY/natsio/prometheus-nats-exporter:0.15.0
```

NATS Server Config Reloader (natsio/nats-server-config-reloader:0.14.2):

```bash
docker pull natsio/nats-server-config-reloader:0.15.1
docker tag natsio/nats-server-config-reloader:0.15.1 YOURREPOSITORY/natsio/nats-server-config-reloader:0.15.1 
docker push YOURREPOSITORY/natsio/nats-server-config-reloader:0.14.2
```

NATS Server (nats:2.10.14-alpine):

```bash
docker pull nats:2.10.20-alpine
docker tag nats:2.10.20-alpine YOURREPOSITORY/nats:2.10.20-alpine
docker push YOURREPOSITORY/nats:2.10.16-alpine
```

Telegraf (docker.io/library/telegraf:1.30-alpine):

```bash
docker pull library/telegraf:1.30-alpine
docker tag library/telegraf:1.30-alpine YOURREPOSITORY/library/telegraf:1.30-alpine
docker push YOURREPOSITORY/library/telegraf:1.30-alpine
```

Curl (curlimages/curl:8.6.0):

```bash
docker pull curlimages/curl:8.6.0
docker tag curlimages/curl:8.6.0 YOURREPOSITORY/curlimages/curl:8.6.0
docker push YOURREPOSITORY/curlimages/curl:8.6.0
```

Superstream Engine (superstreamlabs/superstream-data-plane-be:latest):

```bash
docker pull superstreamlabs/superstream-data-plane-be:latest
docker tag superstreamlabs/superstream-data-plane-be:latest YOURREPOSITORY/superstreamlabs/superstream-data-plane-be:latest
docker push YOURREPOSITORY/superstreamlabs/superstream-data-plane-be:latest
```

Syslog (linuxserver/syslog-ng:4.5.0):

```bash
docker pull linuxserver/syslog-ng:4.5.0
docker tag linuxserver/syslog-ng:4.5.0 YOURREPOSITORY/linuxserver/syslog-ng:4.5.0 
docker push YOURREPOSITORY/linuxserver/syslog-ng:4.5.0
```

## Getting started

### 1. Download Helm Chart

Download the Superstream Helm chart from the official source as described above.&#x20;

### 2. Publish to Private Environments

Once downloaded, publish the chart to your private Helm chart repositories. This step ensures that you maintain control over the versions and configurations of the chart used in your deployments.&#x20;

Docker Image Names: You must change the Docker image names within the Helmfile to reflect those stored in your private Docker registries. This customization is crucial for ensuring that your deployments reference the correct resources within your secure environment:

### 3. Configure All Services to Use a Private Docker Repository and Private PullSecret

For easiness create/use `custom_values.yaml` file and add `global.image` section values, an example can be found [here](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/custom\_values.yaml):

````yaml
```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: "" # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: "" # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: "" # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: true
  
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
