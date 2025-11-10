---
title: Confluent Cloud user creation
---

{% tabs %}
{% tab title="Automatic" %}
### **Step 1: Create a new Service Account**

1. In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**
2. **Name** the service account "`Superstream`" (The Service account name must include the word "Superstream".)
3. Set account type to "None"
4. Permissions:
   1. **Organization ->**  Add role assignment(top right) and add the following permissions:
      1. `BillingAdmin` (\* Optional)
      2. `ResourceKeyAdmin` (\* Optional)\
         <mark style="color:red;">**You can limit**</mark> the scope of this permission by explicitly setting `EnvironmentAdmin` in a specific environment. Once that setting exists in one particular environment, the `ResourceKeyAdmin` permission will no longer control the entire organization.
      3. `MetricsViewer` (\* Required)

### Step 2: Create a Cloud Resource Management Key

1. In Confluent Console: Top-right menu -> API Keys -> + Add API key
2. Select the Service account
3. Select Cloud Resource Management
4. Use the created key in the Superstream console
{% endtab %}

{% tab title="Manual" %}
### **Step 1: Create a new Service Account**

1. In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**
2. **Name** the service account "`Superstream`" (The Service account name must include the word "Superstream".)
3. Set account type to "None"
4. Permissions:
   1. **Organization ->**  Add role assignment(top right) and add the following permissions:
      1. `BillingAdmin` (\* Optional)
      2. `MetricsViewer` (\* Required)

### Step 2: Create a Cloud Resource Management Key

1. In Confluent Console: Top-right menu -> API Keys -> + Add API key
2. Select the Service account
3. Select Cloud Resource Management
4. Use the created key in the Superstream console

### Step 3: Create a Cluster-level API key

1. In Confluent Console: Main menu -> Cluster -> API Keys -> + Add API key
2. If ACLs are enabled, please use the following:

For READ+WRITE (Superstream to perform actions)

```
// cluster ACLs
{"CLUSTER", "kafka-cluster", "LITERAL", "ALTER_CONFIGS", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "CREATE", "ALLOW"}

// consumers groups ACLs
{"GROUP", "*", "LITERAL", "DELETE", "ALLOW"}
{"GROUP", "*", "LITERAL", "DESCRIBE", "ALLOW"}
{"GROUP", "*", "LITERAL", "READ", "ALLOW"}

// topics ACLs
{"TOPIC", "*", "LITERAL", "ALTER", "ALLOW"}
{"TOPIC", "*", "LITERAL", "ALTER_CONFIGS", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DELETE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DESCRIBE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"}
{"TOPIC", "*", "LITERAL", "READ", "ALLOW"}
{"TOPIC", "*", "LITERAL", "WRITE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "CREATE", "ALLOW"}
```

For READ only (Superstream to analyze only)

```
{"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "CREATE", "ALLOW"}

// consumers groups ACLs
{"GROUP", "*", "LITERAL", "DESCRIBE", "ALLOW"}
{"GROUP", "*", "LITERAL", "READ", "ALLOW"}

// topics ACLs
{"TOPIC", "*", "LITERAL", "DESCRIBE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"}
{"TOPIC", "*", "LITERAL", "READ", "ALLOW"}
{"TOPIC", "*", "LITERAL", "WRITE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "CREATE", "ALLOW"}
```
{% endtab %}
{% endtabs %}
