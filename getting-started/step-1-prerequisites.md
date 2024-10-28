# Step 1: Prerequisites

### **1. Create a user and/or a Vendor API key**

Superstream platform is compatible with all Kafka authentication methods, such as:

* SASL/SCRAM
* SASL/GSSAPI
* SASL/PLAIN
* ACL
* RBAC

{% tabs %}
{% tab title="Confluent Cloud" %}
For connecting Confluent Cloud clusters to Superstream, two types of API keys are required to be created:&#x20;

### Cluster connectivity key

#### Create one using REST calls:

#### Create one using Confluent Console:

1. Home -> Environments -> \<environment name> -> \<cluster name> -> API Keys

<figure><img src="../.gitbook/assets/Screenshot 2024-10-15 at 13.42.03.png" alt="" width="172"><figcaption></figcaption></figure>

2.  Click on "+ Add key"

    <figure><img src="../.gitbook/assets/Screenshot 2024-10-15 at 13.43.29.png" alt="" width="375"><figcaption></figcaption></figure>
3. In the opened walkthrough:
   1. Choose "**Service account**"
   2. "**Create a new one**" named `Superstream`
   3. Define the following rules:
      1. Cluster
         1. `ALTER_CONFIGS`: ALLOW
         2. `DESCRIBE`: ALLOW
         3. `DESCRIBE_CONFIGS`: ALLOW
      2. Consumer Group (For all "\*")
         1. LITERAL, DESCRIBE, ALLOW
         2. LITERAL, READ, ALLOW
   4. "Create" **and save the newly created creds**.
   5. Main menu -> Accounts & access -> Service accounts -> Superstream
      1.  Add the following role assignments:

          **For each designated organization:**

          * `BillingAdmin`

          **For each designated environment (Environment level):**

          * M`etricsViewer`
          * `DataDiscovery`
          * `Operator`

          **For each designated cluster (Cluster level):**

          * `CloudClusterAdmin`
      2. For each designated **cluster** -> **Topics**
         1. `DeveloperRead`: All topics
         2. `DeveloperManage`: All topics

### Vendor key

1. Head over to **Main menu** -> **API Keys** -> "**+ Add API key**" and perform the following:

<figure><img src="../.gitbook/assets/Screenshot 2024-10-06 at 20.36.31.png" alt="" width="375"><figcaption></figcaption></figure>

***

<figure><img src="../.gitbook/assets/Screenshot 2024-10-06 at 20.37.52.png" alt="" width="375"><figcaption></figcaption></figure>

***

<figure><img src="../.gitbook/assets/Screenshot 2024-10-15 at 14.10.00.png" alt="" width="375"><figcaption></figcaption></figure>

***

<figure><img src="../.gitbook/assets/Screenshot 2024-10-06 at 20.39.28.png" alt="" width="375"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Apache Kafka" %}
For effective functioning, a user or token requires the following permissions:

* Cluster-level:
  * Describe all topics, List all topics, Describe configs, Describe cluster
* Topic-level:
  * Read: All topics
  * Alter: All topics
  * Delete: All topics
  * Describe: All topics
  * Alter: All topics
  * AlterConfigs: All topics
  * DescribeConfigs: All topics
  * <mark style="color:red;">**Read, Create, and Write: single topic named**</mark>** `superstream.metadata`** (A dedicated Superstream topic with infinite retention and a single partition).
* Consumer group-level:
  * Describe
  * List Consumer Groups

ACL statement that grants `read` access to a user named Superstream for `all topics` in the Kafka cluster:

{% code overflow="wrap" %}
```bash
kafka-acls --bootstrap-server : -add --allow-principal User:Superstream --operation read --topic '' --group '' --command-config <PATH_TO_CRED_FILE>
```
{% endcode %}

ACL statement that grants `describe` access to a user named Superstream for `all topics` in the Kafka cluster:

{% code overflow="wrap" %}
```bash
kafka-acls --bootstrap-server : -add --allow-principal User:Superstream --operation Describe --topic '*' --command-config <PATH_TO_CRED_FILE>
```
{% endcode %}

ACL statement that grants `DescribeConfigs` access to a user named Superstream for `all topics` in the Kafka cluster:

{% code overflow="wrap" %}
```bash
kafka-acls --bootstrap-server <URL>:<PORT>  -add --allow-principal User:Superstream --operation DescribeConfigs --topic '*' --command-config <PATH_TO_CRED_FILE>
```
{% endcode %}
{% endtab %}

{% tab title="AWS MSK " %}

{% endtab %}
{% endtabs %}

### **2.** _For hybrid deployments only._ **Network configuration**

**Ports**: The Superstream engine should be able to communicate with each designated Kafka cluster through the following ports:

* Port 9092 is used for data communication between the Superstream controller and the designated Kafka cluster.
* Port 9999 facilitates JMX and monitoring communication between the Superstream controller and the designated Kafka cluster.
* Port 4222 to enable secure communication for metadata transfer between the on-prem Superstream data plane to the external Superstream control plane

### 3. _For hybrid deployments only._ Engine deployment-related

1. Kubernetes Cluster: You need an up-and-running Kubernetes cluster. If you don't have one, you can create a cluster on platforms like Google Kubernetes Engine (GKE), Amazon EKS, Azure AKS, or Minikube for local development.
2. [kubectl](https://kubernetes.io/docs/tasks/tools/): The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. Install kubectl if you haven't already.
3. [Helm](https://helm.sh/docs/intro/install/): Helm is a package manager for Kubernetes that facilitates the deployment and management of applications. Install Helm if it's not already set up.
4. A defined default storage class. In case you can't define one, please use this [appendix](step-1-prerequisites.md#appendix-d-custom-changes-to-the-helmfile)
5. `Account ID` and `Activation Token`. To be received by the Superstream team.
6. Fill in the "[Environment readiness](https://docs.google.com/spreadsheets/d/1z-IRt6jBhMpL-T9XhL0k1hoPHgAZnlSoPh0ay2ymses/edit?usp=sharing)" checklist

**K8S resources:**

* `CPU`: A minimum of 4 CPUs.
* `RAM`: A minimum of 8 GB.

**Storage Requirements:**&#x20;

Default Storage Class: A default storage class must be configured and available in the Kubernetes cluster to dynamically provision storage as required by the application.

### 4. _For hybrid deployments only._ Fill out the Environment Readiness Checklist

The Superstream Environment Readiness Checklist ensures that everything is set up for the successful deployment of the SSM engine with reliability and resilience.

You can find the sheet [here](https://docs.google.com/spreadsheets/d/1z-IRt6jBhMpL-T9XhL0k1hoPHgAZnlSoPh0ay2ymses/edit?gid=0#gid=0). Please make a copy and share the link with your project manager.
