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

**Step 2:**  Fill in the form with your key information

{% tabs %}
{% tab title="Confluent" %}
1. Head over to your **Confluent Console -> Right-side Menu -> API Keys**
2. On the right-top corner, click on **"**[**+ Add API key**](https://confluent.cloud/settings/api-keys/create)**"**
   1. Choose **"My Account"**
   2. The scope should be **"Cloud resource management"**
   3. Give the new key a name of your choice
3. **Paste** the generated credentials in the **Superstream Console**

{% embed url="https://youtu.be/d8eNgxFkc4M" %}
{% endtab %}

{% tab title="AWS MSK" %}
**AWS MSK** users, please create a programmable user using this IAM policy:

{% code lineNumbers="true" %}
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1716722017278",
      "Action": [
        "billing:GetBillingData",
        "billing:GetBillingDetails",
        "kafka:ListClustersV2",
        "ce:GetCostAndUsageWithResources",
        "ce:GetCostAndUsage"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```
{% endcode %}
{% endtab %}

{% tab title="Aiven" %}
To create an authentication token on the [Aiven Console](https://console.aiven.io/):

1. Click the **User information** icon in the top right and select **Tokens**.
2. Click **Generate token** to open the creation modal.
3. Click **Generate token**.
4. **Copy** and save your token in the modal opened in step 2.
5. Attach the key to one or more Kafka clusters by selecting them in the "**Connections**" select box
{% endtab %}
{% endtabs %}
