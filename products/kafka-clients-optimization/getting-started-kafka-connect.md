---
description: >-
  This guide shows how to integrate superstream-client into Kafka Connect so
  your connectors.
---

# Getting started - Kafka Connect

### What you’ll set up

* Kafka Source Connector with the **superstream-client** Java package
* Required environment variables
* Optional environment variables

#### 1. Download the SuperClient package

Always ensure you are using the most up-to-date version of the SuperClient package from **Maven Central**, **GitHub Releases**, or your internal artifact repository.

**From Maven Central** – Add the [dependency](https://central.sonatype.com/artifact/ai.superstream/superstream-clients) in your build system and include the JAR in your Kafka Connect image or plugin path.

Or,

**From GitHub** – [Download the JAR](https://github.com/superstreamlabs/superstream-clients-java/releases) and place it in the Kafka Connect plugin path (for example `/opt/kafka/plugins/superstream-clients/`).

#### 2. Set Up Environment Variables

**Required Environment Variable:**  Attaches the Superstream Java agent to the Connect JVM. Ensure the path matches the actual location of the JAR inside the container/host.

```
- name: JAVA_TOOL_OPTIONS
  value: "-javaagent:/opt/kafka/plugins/superstream-clients/superstream-clients-<latest>.jar"
```

#### **Optional Variables**

* **SUPERSTREAM\_LATENCY\_SENSITIVE**\
  Set to `"true"` to prevent any modifications to `linger.ms` values.
* **SUPERSTREAM\_DISABLED**\
  Set to `"true"` to disable all Superstream optimizations.
* **SUPERSTREAM\_DEBUG**\
  Set to `"true"` to enable debug logs.

> ⚠️ If your environment already uses `JAVA_TOOL_OPTIONS`, append the `-javaagent=...` flag without overwriting existing options.
