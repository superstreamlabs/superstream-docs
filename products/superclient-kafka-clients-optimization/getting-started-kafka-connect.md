---
description: >-
  This guide shows how to integrate superstream-client into Kafka Connect so
  your connectors.
---

# Getting started - Kafka Connect

### What you’ll set up

* Kafka Connect with the **superstream-client** Java agent
* Required environment variables for the agent

#### Artifact & Version

Use the latest **Superstream client** artifact. Always ensure you are using the most up-to-date version from **Maven Central**, **GitHub Releases**, or your internal artifact repository.

* **Maven Central** – add the dependency in your build system and include the JAR in your Kafka Connect image or plugin path.
* **GitHub Releases** – download the JAR and place it in the Kafka Connect plugin path (for example `/opt/kafka/plugins/superstream-clients/`).

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
