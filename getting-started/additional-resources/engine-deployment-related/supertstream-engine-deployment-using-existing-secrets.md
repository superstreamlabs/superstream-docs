# Supertstream Engine Deployment using existing secrets

When the `ACTIVATION_TOKEN` cannot be exposed in the `values.yaml` file, it is possible to provide it to the Engine using a pre-created Kubernetes secret containing the relevant data. Follow these steps to create and configure the secret:

#### **Create Kubernetes Secret**

Use the following command to create a Kubernetes secret with the required data:

```
kubectl create secret generic superstream-creds --from-literal=ACTIVATION_TOKEN=<TOKEN> --from-literal=ENCRYPTION_SECRET_KEY=<RANDOM_STRING_OF_32_CHAR> -n superstream
```

#### **Generate `ENCRYPTION_SECRET_KEY`**

To create the `ENCRYPTION_SECRET_KEY`, run the following command:

```
openssl rand -hex 16
```

#### **Specify Existing Secret in `custom_values.yaml`**

Indicate that you are using an existing secret by adding the following lines to your `custom_values.yaml` file:

```yaml
superstreamEngine:  
  secret:
    useExisting: true
```

#### **Final Configuration**

After configuring the secret, your overall configuration should look like this:

```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                    # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""          # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""    # Enter the activation token required for services or resources that need an initial token for activation or authentication.
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
superstreamEngine:  
  secret:
    useExisting: true
```

#### **Deploy**

{% code overflow="wrap" lineNumbers="true" %}
```bash
helm repo add superstream https://k8s.superstream.ai/ --force-update && helm install superstream superstream/superstream -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}
