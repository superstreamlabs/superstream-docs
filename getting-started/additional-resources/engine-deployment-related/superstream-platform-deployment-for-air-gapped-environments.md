---
description: >-
  This guide provides instructions for deploying the Superstream Platform for
  air-gapped environments.
hidden: true
---

# Superstream Platform deployment for Air-Gapped environments

{% hint style="info" %}
This manual requires a connection to the internet to pull container images and helm charts.
{% endhint %}

### Step 1: Create Secrets with Randomly Generated Passwords for SSM

To create a secret for the Superstream with randomly generated passwords, run the following command:

```bash
kubectl create secret generic superstream-creds-control-plane \
  --from-literal=postgres-password=$(openssl rand -base64 16 | tr -dc 'a-zA-Z0-9') \
  --from-literal=password=$(openssl rand -base64 16 | tr -dc 'a-zA-Z0-9') \
  --from-literal=repmgr-password=$(openssl rand -base64 16 | tr -dc 'a-zA-Z0-9') \
  --from-literal=admin-password=$(openssl rand -base64 16 | tr -dc 'a-zA-Z0-9') \
  --from-literal=superstream-admin-password=$(openssl rand -base64 16 | tr -dc 'a-zA-Z0-9') \
  --from-literal=control-plane-token=$(openssl rand -base64 48 | tr -dc 'a-zA-Z0-9' | head -c32) \
  --from-literal=encryption-secret-key=$(openssl rand -base64 48 | tr -dc 'a-zA-Z0-9' | head -c32) \
  --from-literal=jwt-secret-key=$(openssl rand -base64 48 | tr -dc 'a-zA-Z0-9' | head -c32) \
  --from-literal=jwt-api-secret-key=$(openssl rand -base64 48 | tr -dc 'a-zA-Z0-9' | head -c32) \
  -n superstream
```

{% hint style="danger" %}
**Important:** The secret name `superstream-creds-control-plane` **cannot be changed** in the current release. This will be fixed in an upcoming release.
{% endhint %}

{% hint style="info" %}
The following keys should have a length of 32 characters:
{% endhint %}

* `encryption-secret-key`
* `jwt-secret-key`
* `jwt-api-secret-key`
* `control-plane-token`

### Step 2: Configure Environment Tokens

For a more straightforward configuration, create a `custom_values.yaml` file and edit the following values:

```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                    # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""          # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""    # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: true
  onPrem: true  
  ## If your environment uses a proxy server, uncomment the lines below and replace the URL with your proxy server's address.
  proxy:
    enabled: false
    proxyUrl: "https://your-proxy-server"

############################################################
# NATS config
############################################################
# NATS HA Deployment. Default "true"
nats:
  config:
    cluster:
      enabled: true
# NATS storageClass configuration. The default is blank "".
    jetstream:
      fileStore:
        pvc:
          storageClassName: ""
############################################################
# Telegraf config
############################################################
# Telegraf custom environment variables configuration.
# telegraf:
#   env:
#   - name: NO_PROXY
#     value: "10.0.0.0/8,8.8.8.8"
```

{% hint style="info" %}
### Proxy Configuration

If your environment requires a proxy server to connect to external services, set the `global.proxy.enabled` variable to `true` and provide the `global.proxy.proxyUrl` in the `custom_values.yaml` file. This configuration ensures that all critical services route traffic through the specified proxy. Additionally, make sure your proxy server permits connectivity to the following endpoints:

* Prometheus: `https://prometheus.mgmt.superstream.ai`
* Loki: `https://loki.mgmt.superstream.ai`
* Stigg: `https://api.stigg.io`
{% endhint %}

### Step 3: Deployment Instructions

To deploy the Superstream, run the following command:

```bash
helm repo add superstream-onprem https://k8s-onprem.superstream.ai/ --force-update && \
helm upgrade --install superstream superstream-onprem/superstream-onprem -f custom_values.yaml --create-namespace --namespace superstream --wait
```

### Step 4: Configure valid FQDN records

To use the Superstream User Interface, the following two FQDN records should be exposed under the same `domain`.

* Expose the Superstream Control Plane service. Using `superstream-api` at the beginning of the configured FQDN is a hard requirement. \
  Example: "superstream-api.example.com"
* Expose the Superstream Control Plane UI service. Example: `superstream-app.example.com`
* Log in to the Superstream UI and connect your first Kafka cluster.

Follow these steps to successfully configure and deploy your Superstream Control Plane environment.
