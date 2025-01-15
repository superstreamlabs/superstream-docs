# Step 1: Preparations

**1. Create a user and/or a Vendor API key**

Superstream platform is compatible with all Kafka authentication methods, such as:

* SASL/SCRAM
* SASL/GSSAPI
* SASL/PLAIN
* ACL
* RBAC

{% tabs %}
{% tab title="AWS MSK " %}
**Step 1: Create a new policy**

Log in to the AWS Console and navigate to the **IAM** section to **create a new policy** with the permissions below: (Copy and paste)

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
                "ec2:DescribeInstanceTypes",
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
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:SimulatePrincipalPolicy",
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

#### Step 2: If you are using an IAM Role <a href="#create-api-key-using-iam-role" id="create-api-key-using-iam-role"></a>

Create a new role with a trusted entity type: `Custom trust policy`

<figure><img src="https://docs.superstream.ai/~gitbook/image?url=https%3A%2F%2F2184988900-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252Fv2riflmHwiTSB6UVhN6P%252Fuploads%252FMLENY4Xrg4wvqq3NZKgt%252Fimage-20240906-122035.png%3Falt%3Dmedia%26token%3D6469e22e-977f-4e88-85c8-f1030f0b15fb&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=8c1f352e&#x26;sv=1" alt=""><figcaption></figcaption></figure>

The "Principal" value will be provided by the Superstream team

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

#### Step 3: Attach the policy created above to the role.

#### Step 4: Add the following AWS-managed policy to the IAM Role:&#x20;

* `AWSBillingReadOnlyAccess`

#### Step 2: If you are using an IAM User <a href="#create-api-key-using-iam-user" id="create-api-key-using-iam-user"></a>

Attach the policy created above to the AWS IAM User and use ACCESS KEY to create the API Key

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### Step 3: Add the following AWS-managed policy to the IAM  User:&#x20;

* `AWSBillingReadOnlyAccess`
{% endtab %}

{% tab title="Confluent Cloud" %}
For connecting Confluent Cloud clusters to Superstream, two types of API keys are required to be created:&#x20;

### Step 1: Create a new Confluent service account

In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**

<figure><img src="../.gitbook/assets/Screenshot 2025-01-14 at 10.02.26.png" alt=""><figcaption></figcaption></figure>

In the "Add service account" wizard:

1. **Name** the service account "`Superstream`"
2. Permissions ("+ Add role assignment"):
   1. For each **organization**: `BillingAdmin` and `MetricsViewer`
   2. For each **environment:** `MetricsViewer` ,`DataDiscovery`, `Operator`&#x20;
      1. For **environment** -> **Schema Registry**
         1. Select resource: `All schema subjects`&#x20;
         2. Select role: `ResourceOwner`&#x20;
   3. For each **cluster:** `CloudClusterAdmin` , `MetricsViewer`
      1. For each designated **cluster** -> **Topics**
         1. `DeveloperRead`: All topics
         2. `DeveloperManage`: All topics
      2. For each designated **cluster** -> **Consumer Groups**
         1. Read all `Consumer groups`

### Step 2: Create a Confluent Cloud Resource Management Key

In Confluent Console: Top-right menu -> API Keys -> + Add API key

Follow the following steps:

<div align="left"><figure><img src="../.gitbook/assets/Screenshot 2024-10-06 at 20.37.52.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../.gitbook/assets/Screenshot 2024-10-15 at 14.10.00.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../.gitbook/assets/Screenshot 2024-10-06 at 20.39.28.png" alt="" width="375"><figcaption></figcaption></figure></div>

Create <mark style="color:red;">**and save the newly created credentials using the cluster name**</mark><mark style="color:red;">.</mark>

### Step 3: Create a dedicated API key per cluster

In Confluent Console: Left menu -> Home -> Environments -> `<environment name>` -> `<cluster name>` -> API Keys

<div align="left"><figure><img src="../.gitbook/assets/Screenshot 2024-10-15 at 13.42.03.png" alt="" width="172"><figcaption></figcaption></figure></div>

Click on "**+ Add key**"

<div align="left"><figure><img src="../.gitbook/assets/Screenshot 2024-10-15 at 13.43.29.png" alt="" width="375"><figcaption></figcaption></figure></div>

1. Choose "**Service account**" -> "`Superstream`" (The one we created in Step 1)
2. ACLs:
   1. Cluster
      1. `ALTER_CONFIGS`: ALLOW
      2. `DESCRIBE`: ALLOW
      3. `DESCRIBE_CONFIGS`: ALLOW
   2. Consumer Group
      1. Rule 1:
         1. Consumer group ID: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `Delete`
         4. Permission: `ALLOW`
      2. Rule 2:
         1. Consumer group ID: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `Describe`
         4. Permission: `ALLOW`
      3. Rule 3:
         1. Consumer group ID: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `Read`
         4. Permission: `ALLOW`&#x20;
   3. Topic
      1. Rule 1:
         1. Topic name: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `ALTER`
         4. Permission: `ALLOW`
      2. Rule 2:
         1. Topic name: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `ALTER_CONFIGS`
         4. Permission: `ALLOW`
      3. Rule 3:
         1. Topic name: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `DELETE`
         4. Permission: `ALLOW`
      4. Rule 4:
         1. Topic name: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `DESCRIBE`
         4. Permission: `ALLOW`
      5. Rule 5:
         1. Topic name: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `DESCRIBE_CONFIGS`
         4. Permission: `ALLOW`
      6. Rule 6:
         1. Topic name: `superstream`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `Create`
         4. Permission: `ALLOW`
      7. Rule 7:
         1. Topic name: `*`&#x20;
         2. Pattern type: `LITERAL`&#x20;
         3. Operation: `READ`
         4. Permission: `ALLOW`
3. Create <mark style="color:red;">**and save the newly created credentials using the cluster name**</mark><mark style="color:red;">.</mark>
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
{% endtabs %}
