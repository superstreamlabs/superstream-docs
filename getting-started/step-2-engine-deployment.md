---
description: This guide explaines how to deploy a local Superstream engine.
---

# Step 2: Engine deployment

{% hint style="warning" %}
If your environment is completely isolated from the external communication, \
please use [this procedure](../procedures/engine-deployment-related/superstream-engine-deployment-for-environments-with-a-local-container-registry.md).
{% endhint %}

{% hint style="warning" %}
This step is only necessary for **local engine deployment** and is not required for fully managed accounts.
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

{% tabs %}
{% tab title="Without TLS (Default)" %}
### 1. Configure Environment Tokens

Create a `custom_values.yaml` file and edit the relevant values (An example can be found [here](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/custom_values.yaml))

{% hint style="warning" %}
{% code title="custom_values.yaml" overflow="wrap" lineNumbers="true" %}
```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""               # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""                 # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""           # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: true
############################################################
# NATS config
############################################################
# NATS HA Deployment. Default "true"
nats:
  config:
    cluster:
      enabled: true
# NATS storageClass configuration. Default is blank "".
    jetstream:
      fileStore:
        pvc:
          storageClassName: ""
    nats:
      port: 4222
      tls:
        enabled: false
        # set secretName in order to mount an existing secret to dir
        secretName: ""
        localCa:
          enabled: false
          secretName: ""          
############################################################
# Kafka Autoscaler config
############################################################
# Optional service to automatically scale the Kafka cluster up/down based on CPU and memory metrics  
autoScaler:
  enabled: true
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
{% endtab %}

{% tab title="With TLS" %}
This guide provides step-by-step instructions for deploying the Superstream Engine with TLS-enabled NATS. The setup includes creating necessary secrets, configuring trust for CA certificates, and aligning Helm chart values for deployment.

**Prerequisites**

1. **TLS Certificate Files**: Ensure you have the `tls.crt` and `tls.key` files for securing NATS communication.
2. **Trusted CA Certificates**: Prepare a `ca-certificates.crt` file that includes the trusted CA certificates for the data plane applications.

### **1. Create the TLS Secret for NATS**

Create a Kubernetes TLS secret in the `superstream` namespace using the provided `tls.crt` and `tls.key` files:

{% code overflow="wrap" %}
```bash
kubectl create secret tls superstream-nats-tls --cert=tls.crt --key=tls.key -n superstream
```
{% endcode %}

This secret secures NATS communication with TLS.

### **2. Create the Trusted CA Secret**

Create a generic Kubernetes secret to store the trusted CA certificates for data plane applications:

{% code overflow="wrap" %}
```bash
kubectl create secret generic nats-ca --from-file=ca-certificates.crt=./ca-certificates.crt -n superstream
```
{% endcode %}

This secret ensures that the applications trust the required CA.

### **3. Update the Helm Chart**

To integrate the secrets into the Superstream Engine deployment, update the Helm chart’s `custom_values.yaml` file. Here is an example configuration:

{% code lineNumbers="true" %}
```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""               # Define the superstream engine name (max 32 characters, lowercase, numbers, '-', '_').
  superstreamAccountId: ""     # Account ID associated with the deployment.
  superstreamActivationToken: "" # Initial token for activation or authentication.
  skipLocalAuthentication: true

############################################################
# NATS config
############################################################
nats:
  config:
    cluster:
      enabled: true
    jetstream:
      fileStore:
        pvc:
          storageClassName: "" # Specify the storage class name for JetStream persistence.
    nats:
      port: 4222
      tls:
        enabled: true
        # Set the TLS secret name
        secretName: "superstream-nats-tls"
        localCa:
          enabled: true
          secretName: "nats-ca"

############################################################
# Kafka Autoscaler config
############################################################
autoScaler:
  enabled: true
```
{% endcode %}

**4. Deploy the Helm Chart**

Go to the `custom_values.yaml` directory and run:

{% code overflow="wrap" %}
```bash
helm repo add superstream https://k8s.superstream.ai/ --force-update && helm upgrade --install superstream superstream/superstream -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}

#### **Notes** <a href="#notes" id="notes"></a>

* Ensure the `tls.crt` and `tls.key` files are valid and signed by a trusted CA.
* The `ca-certificates.crt` file should include all necessary trusted CA certificates for the data plane applications.
* Align the secret names in `custom_values.yaml` with the names created in the Kubernetes namespace.
{% endtab %}
{% endtabs %}

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
