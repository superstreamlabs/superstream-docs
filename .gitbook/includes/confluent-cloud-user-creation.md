---
title: Confluent Cloud user creation
---

### **Step 1: Create a new Service Account**

1. In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**
2. **Name** the service account "`Superstream`" (The Service account name must include the word "Superstream".)
3. Set account type to "None"
4. Click on the **organization ->**  Add role assignment(top right) and add the following permissions:
   1. `BillingAdmin` - on the organization level
   2. `ResourceKeyAdmin` - on the organization level
   3. `MetricsViewer`&#x20;
5.  In case you want Superstream to connect only with clusters in a specific environment, please grant

    `EnvironmentAdmin` - for each environment you want to connect with Superstream
6. **Optional:** In case you want Superstream to connect only with specific clusters, please grant `CloudClusterAdmin` for each such cluster, instead of granting `EnvironmentAdmin` for the entire environment

### Step 2: Create a Cloud Resource Management Key

1. In Confluent Console: Top-right menu -> API Keys -> + Add API key
2. Select the Service account
3. Select Cloud Resource Management
4. Use the created key in the Superstream console
