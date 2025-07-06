# Supertstream Agent Deployment using custom resource limits

Superstream Agent is deployed with default resource limits designed to ensure high performance. In some cases, these configured limits may not be sufficient. To address potential performance bottlenecks, you can adjust the resource limits using the procedure outlined below.

#### **Specify desired resource limits in the `custom_values.yaml` .**

To adjust the resource limits for the Superstream Agent Data Plane, add the following configuration:

```yaml
superstreamAgent:  
  resources:
    limits:
      cpu: '8'
      memory: 8Gi
```

To modify the resource limits for Superstream Autoscaller, add the following configuration:

```yaml
autoScaller:  
  resources:
    limits:
      cpu: '8'
      memory: 8Gi
```

#### Example: Final Configuration File

Below is an example of a complete configuration file (`custom_values.yaml`) after setting custom resource limits:

```yaml
############################################################
# GLOBAL configuration for Superstream Agent
############################################################
global:
  agentName: "" # Define the superstream agent name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: "" # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: "" # Enter the activation token required for services or resources that need an initial token for activation or authentication.

superstreamAgent:  
  resources:
    limits:
      cpu: '8'
      memory: 8Gi
```

#### **Deploy**

Once you have updated the `custom_values.yaml` file with your desired resource limits, deploy the Superstream Engine using Helm:

{% code overflow="wrap" lineNumbers="true" %}
```bash
helm repo add superstream-agent https://superstream-agent.k8s.superstream.ai/ --force-update && helm upgrade --install superstream superstream-agent/superstream-agent -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}
