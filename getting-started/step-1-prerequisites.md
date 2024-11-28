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

### Kafka vendor API key

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
  * <mark style="color:red;">**Read, Create, and Write: single topic named**</mark>**&#x20;`superstream.metadata`** (A dedicated Superstream topic with infinite retention and a single partition).
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
#### Kafka vendor API key <a href="#kafka-vendor-api-key-1" id="kafka-vendor-api-key-1"></a>

**Create a new Policy**

Log in to the AWS Console and navigate to the **IAM** section to **create a new policy** with the permissions below:

{% code lineNumbers="true" %}
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EC2VpcEndpoint1",
            "Effect": "Allow",
            "Action": "ec2:CreateVpcEndpoint",
            "Resource": "arn:*:ec2:*:*:vpc-endpoint/*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/AWSMSKManaged": "true"
                },
                "StringLike": {
                    "aws:RequestTag/ClusterArn": "*"
                }
            }
        },
        {
            "Sid": "EC2VpcEndpoint2",
            "Effect": "Allow",
            "Action": "ec2:CreateTags",
            "Resource": "arn:*:ec2:*:*:vpc-endpoint/*",
            "Condition": {
                "StringEquals": {
                    "ec2:CreateAction": "CreateVpcEndpoint"
                }
            }
        },
        {
            "Sid": "EC2VpcEndpoint3",
            "Effect": "Allow",
            "Action": "ec2:DeleteVpcEndpoints",
            "Resource": "arn:*:ec2:*:*:vpc-endpoint/*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/AWSMSKManaged": "true"
                },
                "StringLike": {
                    "ec2:ResourceTag/ClusterArn": "*"
                }
            }
        },
        {
            "Sid": "EC2VpcEndpoint4",
            "Effect": "Allow",
            "Action": "ec2:CreateVpcEndpoint",
            "Resource": [
                "arn:*:ec2:*:*:vpc/*",
                "arn:*:ec2:*:*:security-group/*",
                "arn:*:ec2:*:*:subnet/*"
            ]
        },
        {
            "Sid": "IAM1",
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "kafka.amazonaws.com"
                }
            }
        },
        {
            "Sid": "IAM2",
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/kafka.amazonaws.com/AWSServiceRoleForKafka*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": "kafka.amazonaws.com"
                }
            }
        },
        {
            "Sid": "IAM3",
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/delivery.logs.amazonaws.com/AWSServiceRoleForLogDelivery*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": "delivery.logs.amazonaws.com"
                }
            }
        },
        {
            "Sid": "Kafka",
            "Effect": "Allow",
            "Action": [
                "kafka:UpdateBrokerCount",
                "kafka:DescribeConfiguration",
                "kafka:ListScramSecrets",
                "kafka:ListKafkaVersions",
                "kafka:GetBootstrapBrokers",
                "kafka:ListClientVpcConnections",
                "kafka:UpdateBrokerType",
                "kafka:DescribeCluster",
                "kafka:ListClustersV2",
                "kafka:DescribeClusterOperation",
                "kafka:ListNodes",
                "kafka:ListClusterOperationsV2",
                "kafka:UpdateClusterConfiguration",
                "kafka:ListClusters",
                "kafka:GetClusterPolicy",
                "kafka:DescribeClusterOperationV2",
                "kafka:DescribeClusterV2",
                "kafka:ListReplicators",
                "kafka:ListConfigurationRevisions",
                "kafka:ListVpcConnections",
                "kafka:ListTagsForResource",
                "kafka:GetCompatibleKafkaVersions",
                "kafka:DescribeConfigurationRevision",
                "kafka:UpdateConfiguration",
                "kafka:ListConfigurations",
                "kafka:ListClusterOperations",
                "kafka:TagResource",
                "kafka:UntagResource",
                "kafka:DescribeVpcConnection",
                "kafka:DescribeReplicator"
            ],
            "Resource": "*"
        },
        {
            "Sid": "KafkaCluster",
            "Effect": "Allow",
            "Action": [
                "kafka-cluster:DescribeTransactionalId",
                "kafka-cluster:CreateTopic",
                "kafka-cluster:*Topic*",
                "kafka-cluster:AlterCluster",
                "kafka-cluster:Connect",
                "kafka-cluster:DeleteTopic",
                "kafka-cluster:ReadData",
                "kafka-cluster:DescribeTopicDynamicConfiguration",
                "kafka-cluster:AlterTopicDynamicConfiguration",
                "kafka-cluster:AlterGroup",
                "kafka-cluster:AlterClusterDynamicConfiguration",
                "kafka-cluster:DescribeGroup",
                "kafka-cluster:DescribeClusterDynamicConfiguration",
                "kafka-cluster:DeleteGroup",
                "kafka-cluster:DescribeCluster",
                "kafka-cluster:AlterTopic",
                "kafka-cluster:DescribeTopic",
                "kafka-cluster:WriteData"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Others",
            "Effect": "Allow",
            "Action": [
                "logs:ListLogDeliveries",
                "ec2:DescribeRouteTables",
                "logs:CreateLogDelivery",
                "logs:PutResourcePolicy",
                "logs:UpdateLogDelivery",
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeSubnets",
                "cloudwatch:GetMetricData",
                "ce:GetCostAndUsage",
                "ec2:DescribeVpcAttribute",
                "cloudwatch:ListMetrics",
                "logs:GetLogDelivery",
                "kms:DescribeKey",
                "logs:DeleteLogDelivery",
                "firehose:TagDeliveryStream",
                "kms:CreateGrant",
                "logs:DescribeResourcePolicies",
                "S3:GetBucketPolicy",
                "logs:DescribeLogGroups",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeVpcs",
                "iam:SimulatePrincipalPolicy",
                "iam:GetUser",
                "ce:GetCostAndUsageWithResources",
                "ce:ListTagsForResource",
                "ce:UpdateCostAllocationTagsStatus",
                "ce:ListCostAllocationTags",
                "ce:GetTags"
            ],
            "Resource": "*"
        }
    ]
}
```
{% endcode %}

#### Create API Key using IAM Role <a href="#create-api-key-using-iam-role" id="create-api-key-using-iam-role"></a>

**Create a new role** with a trusted entity type: `Custom trust policy`

<figure><img src="https://docs.superstream.ai/~gitbook/image?url=https%3A%2F%2F2184988900-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252Fv2riflmHwiTSB6UVhN6P%252Fuploads%252FMLENY4Xrg4wvqq3NZKgt%252Fimage-20240906-122035.png%3Falt%3Dmedia%26token%3D6469e22e-977f-4e88-85c8-f1030f0b15fb&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=8c1f352e&#x26;sv=1" alt=""><figcaption></figcaption></figure>

Attach the following policy:

The exact Principal will be given by the Superstream team

```json
{
	"Version": "2012-10-17",
	"Statement": [
	    {
		"Sid": "Statement1",
		"Effect": "Allow",
		"Principal": {
                	"AWS": "arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_ASSIGNED_TO_NODEGROUP>"
                },
		"Action": "sts:AssumeRole"
	    }
	]
}
```

#### Create API Key using IAM User <a href="#create-api-key-using-iam-user" id="create-api-key-using-iam-user"></a>

Attach the new policy to the AWS IAM User and use ACCESS KEY to create the API Key

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### <mark style="color:blue;">For local engines (not fully managed) only:</mark>

### **2. Network configuration**

**Ports**: The Superstream engine should be able to communicate with each designated Kafka cluster through the following ports:

* Port 9092 is used for data communication between the Superstream controller and the designated Kafka cluster.
* Port 9999 facilitates JMX and monitoring communication between the Superstream controller and the designated Kafka cluster.
* Port 4222 to enable secure communication for metadata transfer between the on-prem Superstream data plane to the external Superstream control plane

### 3. Engine deployment-related

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

### 4. Fill out the Environment Readiness Checklist

The Superstream Environment Readiness Checklist ensures that everything is set up for the successful deployment of the SSM engine with reliability and resilience.

You can find the sheet [here](https://docs.google.com/spreadsheets/d/1z-IRt6jBhMpL-T9XhL0k1hoPHgAZnlSoPh0ay2ymses/edit?gid=0#gid=0). Please make a copy and share the link with your project manager.
