---
description: This page describes how to connect Superstream to one or more Kafka clusters
---

# Step 3: Connect Your Kafka Cluster/s

## Option 1: Using Superstream Console

### Step 1: Login to [Superstream Console](https://app.superstrem.ai)

If this is your first login, please use the link included in your welcome email.

### Step 2: Add a new connection

In the upper-right corner, you will find this button:

### &#x20;![](<../.gitbook/assets/Screenshot 2024-05-01 at 13.12.41.png>)

### Step 3: Fill in the details

<div align="left">

<figure><img src="../.gitbook/assets/Screenshot 2024-05-01 at 13.14.31.png" alt=""><figcaption></figcaption></figure>

</div>

Please do not close the window until a message is shown.

## Option 2: During Superstream engine deployment

Every Engine is capable of establishing connections with one or several Kafka clusters simultaneously. To determine the optimal strategy for your setup, it is advisable to seek guidance from the Superstream team.

Kafka clusters should be defined in the `config.yaml` file:

```yaml
connections:
  - name: <connection_name>
    type: <connection_type> # available types : confluent_kafka, confluent_cloud_kafka, MSK, apache_kafka
    hosts: # list of bootstrap servers
      - <bootstrap_server_1>
      - <bootstrap_server_2>
    authentication: # Specify one preferred method for connection
      method:
        noAuthentication:
          enabled: <true_or_false>
        ssl:
          enabled: <true_or_false>
          protocol: SSL
          ca: "<path_to_ca_cert>"
          cert: "<path_to_cert>"
          key: "<path_to_key>"
          insecureSkipVerify: <true_or_false>
        sasl:
          enabled: <true_or_false>
          mechanism: <sasl_mechanism> # available options: "PLAIN", "SCRAM-SHA-512"
          protocol: SASL_SSL
          username: "<username>"
          password: "<password>"
          tls:
            enabled: <true_or_false>
            ca: "<path_to_ca_cert>"
            cert: "<path_to_cert>"
            key: "<path_to_key>"
            insecureSkipVerify: <true_or_false>
```

* `name`: A unique name for the connection to be displayed through Superstream GUI.
* `type`: The type of Kafka. Options include `confluent_kafka`, `confluent_cloud_kafka`, `MSK`, and `apache_kafka`.
* `hosts`: A list of bootstrap servers for the Kafka cluster.

**Authentication:**

* `noAuthentication`: Set `enabled` to `true` if no authentication is required.
* `ssl`: For SSL encryption without SASL authentication.
  * `enabled`: Set to `true` to enable SSL authentication.
  * `protocol`: Should always be `SSL`.
  * `ca`, `cert`, `key`: Paths to your CA certificate, client certificate, and client key files.
  * `insecureSkipVerify`: Set to `true` to bypass server certificate verification (not recommended for production environments).
* `sasl`: For SASL authentication.
  * `enabled`: Set to `true` enable SASL authentication.
  * `mechanism`: The SASL mechanism to use. Options: `PLAIN`, `SCRAM-SHA-512`.
  * `protocol`: Should be `SASL_SSL` for encrypted connections.
  * `username` and `password`: Credentials for SASL authentication.
  * `tls`: Optional TLS configuration for SASL authentication.

If TLS is used with SASL, specify the following:

* `enabled`: Set to `true` to enable TLS.
* `ca`, `cert`, `key`: Paths to your CA certificate, client certificate, and client key files, if required.
* `insecureSkipVerify`: Set to `true` to bypass server certificate verification (not recommended for production environments).

#### Example

Below is an example configuration for a SASL\_SSL authenticated connection:

```yaml
connections:
  - name: my_kafka_connection
    type: confluent_cloud_kafka
    hosts:
      - kafka.example.com:9092
    authentication:
      method:
        sasl:
          enabled: true
          mechanism: PLAIN
          protocol: SASL_SSL
          username: "myUsername"
          password: "myPassword"
          tls:
            enabled: false
```

Replace placeholders (e.g., `<connection_name>`, `<bootstrap_server_1>`) with your actual Kafka connection details. As shown in the examples, Follow the correct indentation and formatting.

For any questions or further assistance, please refer to the official Kafka documentation or reach out to your Kafka provider.

## \*Optional\* Add a vendor API key to gain deeper insights.

You can add an API key to gain deeper insights for eligible Kafka vendors such as Confluent, Aiven, Redpanda, and AWS.

Here's how to do it if you didn't set it up during the initial client connection establishment:

**Step 1:** Head over to Settings -> "[Keys](https://app.superstream.ai/keys)"

<figure><img src="../.gitbook/assets/Screenshot 2024-05-23 at 16.06.03.png" alt=""><figcaption></figcaption></figure>

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

{% embed url="https://youtu.be/d8eNgxFkc4M" %}
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
### Kafka vendor API key

#### Using IAM Role

Log in to the AWS Console and navigate to the **IAM** section to **create a new policy** with the permissions below:

{% code lineNumbers="true" %}
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kafka:*",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeRouteTables",
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeVpcAttribute",
                "kms:DescribeKey",
                "kms:CreateGrant",
                "logs:CreateLogDelivery",
                "logs:GetLogDelivery",
                "logs:UpdateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:ListLogDeliveries",
                "logs:PutResourcePolicy",
                "logs:DescribeResourcePolicies",
                "logs:DescribeLogGroups",
                "S3:GetBucketPolicy",
                "firehose:TagDeliveryStream"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateVpcEndpoint"
            ],
            "Resource": [
                "arn:*:ec2:*:*:vpc/*",
                "arn:*:ec2:*:*:subnet/*",
                "arn:*:ec2:*:*:security-group/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateVpcEndpoint"
            ],
            "Resource": [
                "arn:*:ec2:*:*:vpc-endpoint/*"
            ],
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
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags"
            ],
            "Resource": "arn:*:ec2:*:*:vpc-endpoint/*",
            "Condition": {
                "StringEquals": {
                    "ec2:CreateAction": "CreateVpcEndpoint"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DeleteVpcEndpoints"
            ],
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
            "Effect": "Allow",
            "Action": [
                "ce:GetCostAndUsageWithResources",
                "ce:GetCostAndUsage",
                "cloudwatch:ListMetrics"
            ],
            "Resource": "*"
        }
    ]
}
```
{% endcode %}

**Create a new role** with a trusted entity type: `AWS account`\
Please use the **Superstream AWS** **account ID** (Will be given by the Superstream team)

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Attach the following policy

```
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


{% endtab %}
{% endtabs %}
