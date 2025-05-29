---
title: Confluent Cloud user creation
---

For connecting Confluent Cloud clusters to Superstream, two types of API keys are required to be created:&#x20;

### Step 1: Create a new Confluent service account

In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**

<figure><img src="../assets/Screenshot 2025-01-14 at 10.02.26.png" alt=""><figcaption></figcaption></figure>

In the "Add service account" wizard:

1. **Name** the service account "`Superstream`"
2. Permissions ("+ Add role assignment"):
   1. For each **organization**: `BillingAdmin` , `ResourceKeyAdmin`, and `MetricsViewer`
   2. For each **environment:** `MetricsViewer` ,`DataDiscovery`, `Operator`&#x20;
      1. For **environment** -> **Schema Registry**
         1. Select resource: `All schema subjects`&#x20;
         2. Select role: `ResourceOwner`&#x20;
   3. For each **cluster:** `CloudClusterAdmin` , `MetricsViewer`
      1. For each designated **cluster** -> **Topics**
         1. `DeveloperRead`: All topics
         2. `DeveloperManage`: All topics
      2. For each designated **cluster** -> **Consumer Groups**
         1. Read all `Consumer group`

### Step 2: Create a Confluent Cloud Resource Management Key

In Confluent Console: Top-right menu -> API Keys -> + Add API key

Follow the following steps:

<div align="left"><figure><img src="../assets/Screenshot 2024-10-06 at 20.37.52.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../assets/Screenshot 2024-10-15 at 14.10.00.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../assets/Screenshot 2024-10-06 at 20.39.28.png" alt="" width="375"><figcaption></figcaption></figure></div>

Create <mark style="color:red;">**and save the newly created credentials using the cluster name**</mark><mark style="color:red;">.</mark>
