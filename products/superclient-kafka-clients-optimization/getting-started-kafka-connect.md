---
description: >-
  This guide shows how to integrate superstream-client into Kafka Connect so
  your connectors.
---

# Getting started - Kafka Connect

### What you’ll set up

* Kafka Connect with the **superstream-client** Java agent
* Required environment variables for the agent

### Prerequisites

* Running Kafka Connect (standalone or distributed).
* Access to the Superstream client JAR artifact.

### Artifact & Version

Use the latest Superstream client artifact available from your artifact repository or distribution point. Always ensure you are using the most up‑to‑date version for security and compatibility.

* **Install path suggestion:** `/opt/kafka/plugins/superstream-clients/`
* **Agent JAR:** `superstream-clients-<latest>.jar`

### Required Environment Variables

Add the following environment variables to the **Kafka Connect** process:

<pre><code>- name: JAVA_TOOL_OPTIONS
<strong>  value: "-javaagent:/opt/kafka/plugins/superstream-clients/superstream-clients-&#x3C;latest>.jar"
</strong>
- name: SUPERSTREAM_TOPICS_LIST
  value: "superstream_accounts,superstream_agents"
</code></pre>

**Details**

* `JAVA_TOOL_OPTIONS`: Attaches the Superstream Java agent to the Connect JVM. Ensure the path matches the actual location of the JAR inside the container/host.
* `SUPERSTREAM_TOPICS_LIST`: Comma‑separated list of topics the client will use. Adjust as needed for your environment.

> ⚠️ If your environment already uses `JAVA_TOOL_OPTIONS`, append the `-javaagent=...` flag without overwriting existing options.
