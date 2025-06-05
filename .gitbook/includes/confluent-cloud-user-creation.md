---
title: Confluent Cloud user creation
---

Step 1: Create a new Confluent service account

In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**

<figure><img src="../assets/Screenshot 2025-01-14 at 10.02.26.png" alt=""><figcaption></figcaption></figure>

In the "Add service account" wizard:

1. **Name** the service account "`Superstream`" (The Service account name must include the word "Superstream".)
2. Set account type to "None"
3. Click on each **organization ->**  Add role assignment(top right) and add the following permissions:
   1. `BillingAdmin` - on the organization level
   2. `ResourceKeyAdmin` - on the organization level
4. **Optional**: In case you want Superstream to connect only with clusters in a specific environment, please grant:
   1. `EnvironmentAdmin` - for each environment you want to connect with Superstream
5. **Optional:** In case you want Superstream to connect only with specific clusters, please grant `CloudClusterAdmin` for each such cluster
   1.  A dedicated Cluster API key with the specified ACLs is required for direct integration into the cluster:

       ```
       {"CLUSTER", "kafka-cluster", "LITERAL", "ALTER_CONFIGS", "ALLOW"},
       {"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE", "ALLOW"},
       {"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"},
       {"CLUSTER", "kafka-cluster", "LITERAL", "CREATE", "ALLOW"},

       // Consumer Group ACLs
       {"GROUP", "*", "LITERAL", "DELETE", "ALLOW"},
       {"GROUP", "*", "LITERAL", "DESCRIBE", "ALLOW"},
       {"GROUP", "*", "LITERAL", "READ", "ALLOW"},

       // Topic ACLs
       {"TOPIC", "*", "LITERAL", "ALTER", "ALLOW"},
       {"TOPIC", "*", "LITERAL", "ALTER_CONFIGS", "ALLOW"},
       {"TOPIC", "*", "LITERAL", "DELETE", "ALLOW"},
       {"TOPIC", "*", "LITERAL", "DESCRIBE", "ALLOW"},
       {"TOPIC", "*", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"},
       {"TOPIC", "*", "LITERAL", "READ", "ALLOW"},
       {"TOPIC", "superstream", "LITERAL", "CREATE", "ALLOW"},

       // Superstream topic ACLs
       {"TOPIC", "superstream.", "PREFIXED", "READ", "ALLOW"},
       {"TOPIC", "superstream.", "PREFIXED", "WRITE", "ALLOW"},
       {"TOPIC", "superstream.", "PREFIXED", "DELETE", "ALLOW"},
       {"TOPIC", "superstream.", "PREFIXED", "DESCRIBE", "ALLOW"},
       {"TOPIC", "superstream.", "PREFIXED", "DESCRIBE_CONFIGS", "ALLOW"},
       {"TOPIC", "superstream.", "PREFIXED", "ALTER", "ALLOW"},
       {"TOPIC", "superstream.", "PREFIXED", "ALTER_CONFIGS", "ALLOW"}
       ```

### Step 2: Create a Confluent Cloud Resource Management Key

In Confluent Console: Top-right menu -> API Keys -> + Add API key

Follow the following steps:

<div align="left"><figure><img src="../assets/Screenshot 2024-10-06 at 20.37.52.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../assets/Screenshot 2024-10-15 at 14.10.00.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../assets/Screenshot 2024-10-06 at 20.39.28.png" alt="" width="375"><figcaption></figcaption></figure></div>

Create <mark style="color:red;">**and save the newly created credentials.**</mark>
