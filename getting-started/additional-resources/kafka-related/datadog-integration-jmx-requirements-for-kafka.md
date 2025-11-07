---
description: >-
  To successfully integrate Datadog with Apache Kafka for metrics collection and
  monitoring, JMX (Java Management Extensions) must be enabled.
---

# Datadog Integration: JMX Requirements for Kafka

### Configuration Steps

#### For Self-Managed Apache Kafka

**Requirement:** Native JMX must be enabled on all Kafka brokers

**Steps:**

1. **Set JMX environment variables** before starting Kafka:

```
export JMX_PORT=9999
export KAFKA_JMX_OPTS="-Dcom.sun.management.jmxremote \
  -Dcom.sun.management.jmxremote.authenticate=false \
  -Dcom.sun.management.jmxremote.ssl=false \
  -Djava.rmi.server.hostname=<BROKER_HOSTNAME> \
  -Dcom.sun.management.jmxremote.port=9999 \
  -Dcom.sun.management.jmxremote.rmi.port=9999"
```

2. **For Kubernetes/Strimzi**, add to Kafka custom resource:

```
spec:
  kafka:
    jmxOptions: {}
```

3. **Restart Kafka brokers** to apply changes
4. **Verify JMX is accessible:**

```
telnet <broker-host> 9999
```

#### For AWS MSK (Managed Streaming for Kafka)

**Requirement:** Open Monitoring with Prometheus must be enabled

**Steps:**

1. **Via AWS Console:**
   * Navigate to Amazon MSK console
   * Select your cluster → "Edit monitoring"
   * Enable "Open monitoring with Prometheus"
   * Check "JMX Exporter"
   * Save changes
2. **Via AWS CLI**

```
aws kafka update-monitoring \
  --cluster-arn <CLUSTER_ARN> \
  --current-version <CLUSTER_VERSION> \
  --open-monitoring '{
    "Prometheus": {
      "JmxExporter": {
        "EnabledInBroker": true
      }
    }
  }'
```

**Verification:** MSK exposes metrics on port 11001 (JMX Exporter) after enabling

**Note:** Open Monitoring is offered at no additional cost for MSK clusters

**Important Notes**

* **Network Access**:
  * For self-managed Kafka: Ensure JMX port (9999) is accessible from Datadog agents within your network.
  * For AWS MSK: Ensure security group rules allow inbound traffic on port 11001 (JMX Exporter) from Datadog agent instances.
