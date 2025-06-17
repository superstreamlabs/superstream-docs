# How to deploy and manage Superstream using ArgoCD

**Deployment Process:**

#### **Create Values YAML Files**

* For easiness create `custom_values.yaml` file and edit relevant values, an example can be found [here](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/custom_values.yaml):

{% hint style="info" %}
Edit `global.image` the section in case it's an air-gapped environment.
{% endhint %}

```yaml
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                 # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""       # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
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
############################################################
# NATS config
############################################################
# NATS HA Deployment. Default "true"
nats:
  config:
    cluster:
      enabled: true
      # NATS storageClass configuration. Default blank "".
    jetstream:
      fileStore:
        pvc:
          storageClassName: ""
```

* Official `values.file` with all abilities can be found [here](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/values.yaml).
* Push these `values.yaml` files to your ArgoCD repository.

#### **Deploy Application YAMLs:**

Follow the examples below to deploy the application YAML files. Pay close attention to the comments provided for each:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: superstream
  namespace: argocd
  labels:
    app.kubernetes.io/managed-by: Helm
spec:
  destination:
    server: https://kubernetes.default.svc # Destination cluster
    namespace: superstream # Adjust the destination namespace with the file environments/default.yaml
  ignoreDifferences:
  - group: apps
    jsonPointers:
    - /spec/replicas
    kind: Deployment
  project: default
  sources:
  - chart: superstream
    helm:
      valueFiles:
      # Path to the values files in your ArgoCD repository.
      - $values/kubernetes-values/superstream/custom-values.yaml
    repoURL: https://k8s.superstream.ai/
    targetRevision: 0.4.5 # Adjust chart version from helmfile
  - ref: values
    # Your ArgoCD repository  
    repoURL: git@github.com:superstreamlabs/argocd-yamls.git
    targetRevision: master
```

#### **Sync and Monitor Deployment:**

* Once your values YAML files are pushed and the applications YAML are deployed, navigate to your ArgoCD dashboard.
* Find the application you just deployed and click the 'Sync' button to initiate the deployment process.
* Monitor the deployment status to ensure all components are successfully deployed and running.

<figure><img src="../../../../.gitbook/assets/Screenshot 2024-06-06 at 13.25.57.png" alt=""><figcaption></figcaption></figure>



By following these steps, you should be able to deploy and upgrade the Superstream Engine using ArgoCD successfully. If you have any questions or need further assistance, refer to the documentation or reach out to the support team.
