---
description: Next steps after ensuring all clusters are visible and well-connected
cover: ../../.gitbook/assets/BYOC.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Step 4: Activate

**Congratulations!** Reaching this step means that all your clusters are well connected and continuously being analyzed and monitored by Superstream.

Here is what we are going to do next:

1. Configure Superstream notifications
2. Optimize Superstream automation for each cluster to align with its specific business logic.
3. Activate Superstream automation

### **Let's start:**

#### 1. Configure Superstream notifications

In Superstream Console -> Settings -> System Settings

Select the notifications, channels, and recipients that Superstream should notify.

<figure><img src="../../.gitbook/assets/Screenshot 2025-01-14 at 11.58.05.png" alt=""><figcaption></figcaption></figure>

#### 2. Optimize Superstream automation

In Superstream Console -> Kafka Clusters -> choose a cluster to start with

<figure><img src="../../.gitbook/assets/Screenshot 2025-01-14 at 11.53.27.png" alt=""><figcaption></figcaption></figure>

Each row in the center represents a specific type of optimization that can be configured before activation.

Let's take "**Inactive topics cleanup**" as an example.

{% hint style="warning" %}
Modifying and saving automation business logic does NOT activate the automation.
{% endhint %}

Click on the Gear Wheel ⚙️ icon

<figure><img src="../../.gitbook/assets/Screenshot 2025-01-14 at 12.06.50.png" alt=""><figcaption></figcaption></figure>

Adjust the values to align with your business logic, then click "Save."

#### 3. Activate

Once we finish tuning the optimization logic, we can now click on "Activate."

<figure><img src="../../.gitbook/assets/Screenshot 2025-01-15 at 9.10.51.png" alt=""><figcaption></figcaption></figure>

Clicking "Activate" enables Superstream automation. This means jobs will be automatically triggered and executed after the specified delay time. If notifications are configured correctly, you will receive updates when a new job is created and when it is completed.
