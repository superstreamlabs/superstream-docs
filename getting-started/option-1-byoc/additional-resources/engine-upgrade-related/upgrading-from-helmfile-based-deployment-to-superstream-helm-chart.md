---
description: >-
  This upgrade requires manual steps due to transitioning from a Helmfile-based
  deployment to the new Superstream Helm Chart.
---

# Upgrading From Helmfile based Deployment to Superstream Helm Chart

### Step-by-Step Upgrade Procedure

#### Step 1: Validate Services and Report Generation

Ensure all services are running and there is no report generation.

```bash
$ kubectl get pods -n superstream
NAME                                      READY   STATUS    RESTARTS   AGE
nats-0                                    3/3     Running   0          9m4s
nats-1                                    3/3     Running   0          9m4s
nats-2                                    3/3     Running   0          9m4s
superstream-data-plane-5dbb87fbb4-fjw8k   1/1     Running   0          9m4s
superstream-syslog-6f9855df8c-hl6p8       1/1     Running   0          9m4s
telegraf-6f77bf96d-m8zgx                  1/1     Running   0          9m4s
```

Check the logs for confirmation:

```bash
$ kubectl logs superstream-data-plane-5dbb87fbb4-fjw8k -n superstream
2024-06-06T12:15:40Z INF Superstream is up and running
2024-06-06T12:15:40Z INF version 1.0.84
2024-06-06T12:15:40Z INF pprof server started on port 6060
```

#### Step 2: Scale Down Deployments

Scale down the `superstream-data-plane` and `superstream-syslog` deployments.

```bash
$ kubectl scale deployment superstream-data-plane --replicas=0 -n superstream
deployment.apps/superstream-data-plane scaled
$ kubectl scale deployment superstream-syslog --replicas=0 -n superstream
deployment.apps/superstream-syslog scaled
```

#### Step 3: List and Remove Helm Releases

{% hint style="danger" %}
List the current Helm releases and remove the Nats and Telegraf releases only.
{% endhint %}

```bash
$ helm list -n superstream
NAME       	NAMESPACE     	REVISION	UPDATED                              	STATUS  	CHART            	APP VERSION
nats       	superstream	1       	2024-06-06 14:14:48.560359 +0200 CEST	deployed	nats-1.1.10      	2.10.12
superstream	superstream	1       	2024-06-06 14:14:48.118618 +0200 CEST	deployed	superstream-0.2.1	1.0.32
telegraf   	superstream	1       	2024-06-06 14:14:48.539718 +0200 CEST	deployed	telegraf-1.8.43  	1.30.0

$ helm uninstall nats -n superstream
release "nats" uninstalled

$ helm uninstall telegraf -n superstream
release "telegraf" uninstalled

$ helm list -n superstream
NAME       	NAMESPACE     	REVISION	UPDATED                              	STATUS  	CHART            	APP VERSION
superstream	superstream	1       	2024-06-06 14:14:48.118618 +0200 CEST	deployed	superstream-0.2.1	1.0.32
```

#### Step 4: Configure Environment Tokens

Create and edit a `custom_values.yaml` file with the necessary environment tokens. An example configuration is provided below: ([Link](https://github.com/superstreamlabs/superstream-engine/blob/master/charts/superstream/custom\_values.yaml))

```
############################################################
# GLOBAL configuration for Superstream Engine
############################################################
global:
  engineName: ""                   # Define the superstream engine name within 32 characters, excluding '.', and using only lowercase letters, numbers, '-', and '_'.
  superstreamAccountId: ""         # Provide the account ID associated with the deployment, which could be used for identifying resources or configurations tied to a specific account.
  superstreamActivationToken: ""   # Enter the activation token required for services or resources that need an initial token for activation or authentication.
  skipLocalAuthentication: true

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
```

#### Step 5: Deploy New Superstream Helm Chart

Add the Superstream Helm repository and deploy using the custom values file.

{% code overflow="wrap" %}
```bash
helm repo add superstream https://k8s.superstream.ai/ --force-update && helm upgrade --install superstream superstream/superstream -f custom_values.yaml --create-namespace --namespace superstream --wait
```
{% endcode %}

#### Step 6: Scale Up Deployments

Scale up the `superstream-data-plane` and `superstream-syslog` deployments.

```bash
$ kubectl scale deployment superstream-data-plane --replicas=2 -n superstream
deployment.apps/superstream-data-plane scaled
$ kubectl scale deployment superstream-syslog --replicas=1 -n superstream
deployment.apps/superstream-syslog scaled
```

#### Step 7: Validate the Deployment

Ensure all services are running by listing the Helm releases and verifying their status.

```
$ helm list -n superstream
NAME       	NAMESPACE     	REVISION	UPDATED                              	STATUS	 CHART            	APP VERSION
superstream	superstream	1       	2024-06-06 15:37:34.873419 +0200 CEST	deployed superstream-0.4.6	1.0.84
```

#### Summary

This document outlines the steps to transition from a Helmfile-based deployment to the new Superstream Helm Chart. The process includes validating the current state, scaling down deployments, removing old Helm releases, configuring environment tokens, deploying the new Helm chart, scaling up the deployments, and validating the final state.

By following these steps, you can ensure a smooth transition and maintain the integrity and availability of the Superstream services.

