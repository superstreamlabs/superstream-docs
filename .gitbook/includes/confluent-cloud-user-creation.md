---
title: Confluent Cloud user creation
---

**Step 1: Create a new Confluent service account**

In Confluent Console: Top-right menu -> Accounts & access -> Accounts -> Service Accounts -> **"Add service account"**

<figure><img src="../assets/Screenshot 2025-01-14 at 10.02.26.png" alt=""><figcaption></figcaption></figure>

In the "Add service account" wizard:

1. **Name** the service account "`Superstream`" (The Service account name must include the word "Superstream".)
2. Set account type to "None"
3. Click on each **organization ->**  Add role assignment(top right) and add the following permissions:
   1. `BillingAdmin` - on the organization level
   2. `ResourceKeyAdmin` - on the organization level
   3. `MetricsViewer`&#x20;
4.  In case you want Superstream to connect only with clusters in a specific environment, please grant

    `EnvironmentAdmin` - for each environment you want to connect with Superstream
5. **Optional:** In case you want Superstream to connect only with specific clusters, please grant `CloudClusterAdmin` for each such cluster instead of granting `EnvironmentAdmin` for the entire environment

### Step 2: Create a Confluent Cloud Resource Management Key

In Confluent Console: Top-right menu -> API Keys -> + Add API key

Follow the following steps:

<div align="left"><figure><img src="../assets/Screenshot 2024-10-06 at 20.37.52.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../assets/Screenshot 2024-10-15 at 14.10.00.png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../assets/Screenshot 2024-10-06 at 20.39.28.png" alt="" width="375"><figcaption></figcaption></figure></div>

Create <mark style="color:red;">**and save the newly created credentials.**</mark>
