---
title: JMX rules
---

To collect detailed Kafka JMX metrics, add the following `rules` section to the JMX Exporter YAML configuration. These patterns match Kafka server, network, controller, log, and Java metrics, and convert them into Prometheus-compatible metrics.

Include this full rules list in the configuration to ensure comprehensive metric coverage:

{% code lineNumbers="true" %}
```yaml
rules:
  # Special cases and very specific rules
  - pattern: kafka.server<type=(.+), name=(.+), clientId=(.+), topic=(.+), partition=(.*)><>Value
    name: kafka_server_$1_$2
    type: GAUGE
    labels:
      clientId: "$3"
      topic: "$4"
      partition: "$5"

  - pattern: kafka.server<type=(.+), name=(.+), clientId=(.+), brokerHost=(.+), brokerPort=(.+)><>Value
    name: kafka_server_$1_$2
    type: GAUGE
    labels:
      clientId: "$3"
      broker: "$4:$5"

  - pattern: kafka.server<type=(.+), cipher=(.+), protocol=(.+), listener=(.+), networkProcessor=(.+)><>connections
    name: kafka_server_$1_connections_tls_info
    type: GAUGE
    labels:
      cipher: "$2"
      protocol: "$3"
      listener: "$4"
      networkProcessor: "$5"

  - pattern: kafka.server<type=(.+), clientSoftwareName=(.+), clientSoftwareVersion=(.+), listener=(.+), networkProcessor=(.+)><>connections
    name: kafka_server_$1_connections_software
    type: GAUGE
    labels:
      clientSoftwareName: "$2"
      clientSoftwareVersion: "$3"
      listener: "$4"
      networkProcessor: "$5"

  - pattern: "kafka.server<type=(.+), listener=(.+), networkProcessor=(.+)><>(.+):"
    name: kafka_server_$1_$4
    type: GAUGE
    labels:
      listener: "$2"
      networkProcessor: "$3"

  - pattern: kafka.server<type=(.+), listener=(.+), networkProcessor=(.+)><>(.+)
    name: kafka_server_$1_$4
    type: GAUGE
    labels:
      listener: "$2"
      networkProcessor: "$3"

  # Percent metrics
  - pattern: kafka.(\w+)<type=(.+), name=(.+)Percent\w*><>MeanRate
    name: kafka_$1_$2_$3_percent
    type: GAUGE

  - pattern: kafka.(\w+)<type=(.+), name=(.+)Percent\w*><>Value
    name: kafka_$1_$2_$3_percent
    type: GAUGE

  - pattern: kafka.(\w+)<type=(.+), name=(.+)Percent\w*, (.+)=(.+)><>Value
    name: kafka_$1_$2_$3_percent
    type: GAUGE
    labels:
      "$4": "$5"

  # Generic per-second counters
  - pattern: kafka.(\w+)<type=(.+), name=(.+)PerSec\w*, (.+)=(.+), (.+)=(.+)><>Count
    name: kafka_$1_$2_$3_total
    type: COUNTER
    labels:
      "$4": "$5"
      "$6": "$7"

  - pattern: kafka.(\w+)<type=(.+), name=(.+)PerSec\w*, (.+)=(.+)><>Count
    name: kafka_$1_$2_$3_total
    type: COUNTER
    labels:
      "$4": "$5"

  - pattern: kafka.(\w+)<type=(.+), name=(.+)PerSec\w*><>Count
    name: kafka_$1_$2_$3_total
    type: COUNTER

  # Generic gauges with optional key-value pairs
  - pattern: kafka.(\w+)<type=(.+), name=(.+), (.+)=(.+), (.+)=(.+)><>Value
    name: kafka_$1_$2_$3
    type: GAUGE
    labels:
      "$4": "$5"
      "$6": "$7"

  - pattern: kafka.(\w+)<type=(.+), name=(.+), (.+)=(.+)><>Value
    name: kafka_$1_$2_$3
    type: GAUGE
    labels:
      "$4": "$5"

  - pattern: kafka.(\w+)<type=(.+), name=(.+)><>Value
    name: kafka_$1_$2_$3
    type: GAUGE

  # Histogram-like metrics (summary emulation)
  - pattern: kafka.(\w+)<type=(.+), name=(.+), (.+)=(.+), (.+)=(.+)><>Count
    name: kafka_$1_$2_$3_count
    type: COUNTER
    labels:
      "$4": "$5"
      "$6": "$7"

  - pattern: kafka.(\w+)<type=(.+), name=(.+), (.+)=(.*), (.+)=(.+)><>(\d+)thPercentile
    name: kafka_$1_$2_$3
    type: GAUGE
    labels:
      "$4": "$5"
      "$6": "$7"
      quantile: "0.$8"

  - pattern: kafka.(\w+)<type=(.+), name=(.+), (.+)=(.+)><>Count
    name: kafka_$1_$2_$3_count
    type: COUNTER
    labels:
      "$4": "$5"

  - pattern: kafka.(\w+)<type=(.+), name=(.+), (.+)=(.*)><>(\d+)thPercentile
    name: kafka_$1_$2_$3
    type: GAUGE
    labels:
      "$4": "$5"
      quantile: "0.$6"

  - pattern: kafka.(\w+)<type=(.+), name=(.+)><>Count
    name: kafka_$1_$2_$3_count
    type: COUNTER

  - pattern: kafka.(\w+)<type=(.+), name=(.+)><>(\d+)thPercentile
    name: kafka_$1_$2_$3
    type: GAUGE
    labels:
      quantile: "0.$4"

  # Controller metrics
  - pattern: kafka.controller<type=(ControllerChannelManager), name=(QueueSize), broker-id=(\d+)><>(Value)
    name: kafka_controller_$1_$2_$4
    labels:
      broker_id: "$3"

  - pattern: kafka.controller<type=(ControllerChannelManager), name=(TotalQueueSize)><>(Value)
    name: kafka_controller_$1_$2_$3

  - pattern: kafka.controller<type=(KafkaController), name=(.+)><>(Value)
    name: kafka_controller_$1_$2_$3

  - pattern: kafka.controller<type=(ControllerStats), name=(.+)><>(Count)
    name: kafka_controller_$1_$2_$3

  # Network metrics
  - pattern: kafka.network<type=(Processor), name=(IdlePercent), networkProcessor=(.+)><>(Value)
    name: kafka_network_$1_$2_$4
    labels:
      network_processor: "$3"

  - pattern: kafka.network<type=(RequestMetrics), name=(.+), request=(.+)><>(Count|Value)
    name: kafka_network_$1_$2_$4
    labels:
      request: "$3"

  - pattern: kafka.network<type=(SocketServer), name=(.+)><>(Count|Value)
    name: kafka_network_$1_$2_$3

  - pattern: kafka.network<type=(RequestChannel), name=(.+)><>(Count|Value)
    name: kafka_network_$1_$2_$3

  # Additional server metrics
  - pattern: kafka.server<type=(.+), name=(.+), topic=(.+)><>(Count|OneMinuteRate)
    name: kafka_server_$1_$2_$4
    labels:
      topic: "$3"

  - pattern: kafka.server<type=(ReplicaFetcherManager), name=(.+), clientId=(.+)><>(Value)
    name: kafka_server_$1_$2_$4
    labels:
      client_id: "$3"

  - pattern: kafka.server<type=(DelayedOperationPurgatory), name=(.+), delayedOperation=(.+)><>(Value)
    name: kafka_server_$1_$2_$3_$4

  - pattern: kafka.server<type=(.+), name=(.+)><>(Count|Value|OneMinuteRate)
    name: kafka_server_$1_total_$2_$3

  - pattern: kafka.server<type=(.+)><>(queue-size)
    name: kafka_server_$1_$2

  # Java memory and GC metrics
  - pattern: java.lang<type=(.+), name=(.+)><(.+)>(\w+)
    name: java_lang_$1_$4_$3_$2

  - pattern: java.lang<type=(.+), name=(.+)><>(\w+)
    name: java_lang_$1_$3_$2

  - pattern: java.lang<type=(.*)>

  # Kafka log metrics
  - pattern: kafka.log<type=(.+), name=(.+), topic=(.+), partition=(.+)><>Value
    name: kafka_log_$1_$2
    labels:
      topic: "$3"
      partition: "$4"
```
{% endcode %}

These rules should be added to the JMX Exporter YAML configuration to expose comprehensive metrics for the Kafka broker, controller, network, log, and JVM.
