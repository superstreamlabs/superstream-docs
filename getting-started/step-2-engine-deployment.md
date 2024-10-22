---
description: >-
  This guide explaines how to deploy a Superstream on-prem engine on a
  Kubernetes platform.
---

# Step 2: Engine deployment

{% hint style="warning" %}
If your environment is completely isolated from the public internet, \
please use [this procedure](../procedures/engine-deployment-related/superstream-engine-deployment-for-environments-with-a-local-container-registry.md).
{% endhint %}

## Overview

### The Superstream chart will deploy the following pods:

* 2 Superstream engines
* 2 Superstream auto-scaler instances
* 3 NATS brokers
* 1 Superstream syslog adapter
* 1 Telegraf agent for monitoring

{% hint style="info" %}
It is highly recommended to deploy one engine per environment (dev, staging, prod)
{% endhint %}

## Getting started

### 1. Configure Environment Tokens

Create a `custom_values.yaml` file and edit the relevant values (An example can be found [here](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/custom\_values.yaml))

{% hint style="warning" %}
{% code title="custom_values.yaml" overflow="wrap" lineNumbers="true" %}
```yaml
  The following values are required:
  engineName: "" #Engine name
  superstreamAccountId: ""
  superstreamActivationToken: ""
```
{% endcode %}
{% endhint %}

### 2. Deploy

1. Go to the `custom_values.yaml` directory and run:

{% code overflow="wrap" lineNumbers="true" %}
```bash
helm repo add superstream https://k8s.superstream.ai/ --force-update && helm install superstream superstream/superstream -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}

Deployment verification:

{% code lineNumbers="true" %}
```bash
helm list
```
{% endcode %}

### 3. Expose (When Client connectivity is needed. Not a mandatory requirement)

For client connectivity from outside the Kubernetes environment being used, it is necessary to expose the Superstream engine on port 4222 outside of the Kubernetes cluster where Superstream is deployed.

Here is an example YAML file to illustrate the required `service` configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: superstream-host-external
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: external
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
    service.beta.kubernetes.io/aws-load-balancer-scheme: internet-facing
    service.beta.kubernetes.io/aws-load-balancer-name: superstream-host-external
spec:
  ports:
  - name: superstream-host-external
    port: 4222
    protocol: TCP
    targetPort: 4222
  selector:
    app.kubernetes.io/component: nats
    app.kubernetes.io/instance: nats
    app.kubernetes.io/name: nats
  type: LoadBalancer
```

### 4. Enter Superstream Console

Superstream Console can be found here: [https://app.superstream.ai](https://app.superstream.ai)

## Appendixes

### Appendix A - Non-HA Deployment

For testing purposes only, Superstream can be deployed without HA capabilities. Change to `false` the following parameter in the `values.yaml` file:

```yaml
# NATS HA Deployment. Default "true"
nats:
  config:
    cluster:
      enabled: false
```

### Appendix B - Superstream Update

1. Retrieve the Most Recent Version of the Superstream Helm Chart

```yaml
 helm repo add superstream https://k8s.superstream.ai/ --force-update
```

2. Make sure to use the same values:

```
helm get values superstream --namespace superstream
```

3. Run the Upgrade command:&#x20;

```yaml
helm upgrade --install superstream superstream/superstream -f custom_values.yaml --namespace superstream --wait
```

### Appendix C - Uninstall

**Steps to Uninstall Superstream Engine.**

1. Delete Superstream Engine Helm Releases:

```
helm delete superstream -n <NAMESPACE>
```

2. Remove Persistent Storage Bound to the Engine:

It's crucial to delete the stateful storage linked to the Engine. Ensure you carefully specify the namespace in the command below before executing it:

```
kubectl delete pvc -l app.kubernetes.io/instance=superstream -n <NAMESPACE>
```

### Appendix D - Use Custom StorageClass

**StorageClass definition**

If there is no default storageClass configured for the Kubernetes cluster or there is a need to choose a custom storageClass, it can be done by specifying its name in the `values.yaml` file.

```yaml
# NATS storageClass configuration. The default is blank "".
    jetstream:
      fileStore:
        pvc:
          storageClassName: ""
```



### Appendix E - Deploy Superstream Engine with internal authentication mode on

To enable secure client authentication for the Superstream Engine, edit the`values.yaml`  file and set the `skipLocalAuthentication` parameter to `false`.

```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                   # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: false
```

### Appendix F - Deploy Superstream Engine using labels, tolerations, nodeSelector and etc'

* To inject custom labels into all services deployed by Superstream, utilize the `global.labels` variable.&#x20;

```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                   # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: true
  
  labels:
    tests: ok
```

* To configure tolerations, nodeSelector, and affinity settings for each deployed service, the adjustments in the following example need to be done:

```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                   # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: true
  
superstreamEngine:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"
syslog:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"
telegraf:
  tolerations:
  - key: "app"
    value: "connectors"
    effect: "NoExecute"
nats:
  podTemplate:
    merge:
      spec:
        tolerations:
          - effect: NoSchedule
            key: node-role.kubernetes.io/master
            operator: Exists
```

## Best practices

### Dev / Staging environments

Connecting your Development/Staging Kafka Clusters to Superstream is recommended. This can be done using either one or more dedicated Superstream engines (data planes) for each environment or the same engine connected to the production clusters.
