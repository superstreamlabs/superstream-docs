# Improve Network Efficiency

Apache Kafka is a high-throughput distributed messaging system that powers many real-time applications. However, Kafka's performance can often be constrained by inefficient network usage—especially in high-throughput or multi-region deployments. Improving Kafka’s network efficiency means optimizing how data flows between clients and brokers, reducing bandwidth usage, minimizing latency, and ultimately ensuring cost-effective and reliable data pipelines.

At Superstream, we can make it easier to manage and optimize Kafka networking, particularly through our open-source `superstream-clients-java` library. This guide walks through how to use the library to boost network efficiency when interacting with Kafka.

***

## Superstream Client For Java

A Java library for automatically optimizing Kafka producer configurations based on topic-specific recommendations.

### Overview

Superstream Clients works as a Java agent that intercepts Kafka producer creation and applies optimized configurations without requiring any code changes in your application. It dynamically retrieves optimization recommendations from Superstream and applies them based on impact analysis.

### Supported Libraries

Works with any Java library that depends on `kafka-clients`, including:

* Apache Kafka Clients
* Spring Kafka
* Alpakka Kafka (Akka Kafka)
* Kafka Streams
* Kafka Connect
* Any custom wrapper around the Kafka Java client

### Features

* **Zero-code integration**: No code changes required in your application
* **Dynamic configuration**: Applies optimized settings based on topic-specific recommendations
* **Intelligent optimization**: Identifies the most impactful topics to optimize
* **Graceful fallback**: Falls back to default settings if optimization fails

### Important: Producer Configuration Requirements

When initializing your Kafka producers, please ensure you pass the configuration as a mutable object. The Superstream library needs to modify the producer configuration to apply optimizations. The following initialization patterns are supported:

✅ **Supported (Recommended)**:

{% code overflow="wrap" lineNumbers="true" %}
```java
// Using Properties (recommended)
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
// ... other properties ...
KafkaProducer<String, String> producer = new KafkaProducer<>(props);

// Using a regular HashMap
Map<String, Object> config = new HashMap<>();
config.put("bootstrap.servers", "localhost:9092");
// ... other properties ...
KafkaProducer<String, String> producer = new KafkaProducer<>(config);

// Using Spring's @Value annotations and configuration loading
@Configuration
public class KafkaConfig {
    @Value("${spring.kafka.bootstrap-servers}")
    private String bootstrapServers;
    // ... other properties ...

    @Bean
    public ProducerFactory<String, String> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        // ... other properties ...
        return new DefaultKafkaProducerFactory<>(configProps);
    }
}
```
{% endcode %}

❌ **Not Supported**:

{% code lineNumbers="true" %}
```java
// Using Collections.unmodifiableMap
Map<String, Object> config = Collections.unmodifiableMap(new HashMap<>());
KafkaProducer<String, String> producer = new KafkaProducer<>(config);

// Using Map.of() (creates unmodifiable map)
KafkaProducer<String, String> producer = new KafkaProducer<>(
    Map.of("bootstrap.servers", "localhost:9092")
);

// Using ProducerConfig.originals() which returns an unmodifiable copy
ProducerConfig config = new ProducerConfig(props);
KafkaProducer<String, String> producer = new KafkaProducer<>(config.originals());

// Using KafkaTemplate's getProducerFactory().getConfigurationProperties()
// which returns an unmodifiable map
KafkaTemplate<String, String> template = new KafkaTemplate<>(producerFactory);
KafkaProducer<String, String> producer = new KafkaProducer<>(
    template.getProducerFactory().getConfigurationProperties()
);
```
{% endcode %}

#### Spring Applications

Spring applications that use `@Value` annotations and Spring's configuration loading (like `application.yml` or `application.properties`) are fully supported. The Superstream library will be able to modify the configuration when it's loaded into a mutable `Map` or `Properties` object in your Spring configuration class.

Example of supported Spring configuration:

```yaml
# application.yml
spring:
  kafka:
    producer:
      properties:
        compression.type: snappy
        batch.size: 16384
        linger.ms: 1
```

```java
@Configuration
public class KafkaConfig {
    @Value("${spring.kafka.producer.properties.compression.type}")
    private String compressionType;
    
    @Bean
    public ProducerFactory<String, String> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.COMPRESSION_TYPE_CONFIG, compressionType);
        return new DefaultKafkaProducerFactory<>(configProps);
    }
}
```

#### Why This Matters

The Superstream library needs to modify your producer's configuration to apply optimizations based on your cluster's characteristics. This includes adjusting settings like compression, batch size, and other performance parameters. When the configuration is immutable, these optimizations cannot be applied.

***

### Installation

_Superstream package_: [https://central.sonatype.com/artifact/ai.superstream/superstream-clients-java/overview](https://central.sonatype.com/artifact/ai.superstream/superstream-clients-java/overview)

#### Step 1: Add Superstream Jar to your application

Download from GitHub\
[https://github.com/superstreamlabs/superstream-clients-java/releases](https://github.com/superstreamlabs/superstream-clients-java/releases)

Available also in Maven Central\
[https://central.sonatype.com/artifact/ai.superstream/superstream-clients](https://central.sonatype.com/artifact/ai.superstream/superstream-clients)

#### Step 2: Add Environment Variables

<table data-full-width="true"><thead><tr><th width="341.51953125">ENV</th><th width="107.9453125">Required?</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>SUPERSTREAM_TOPICS_LIST</code></td><td>Yes</td><td>Comma-separated list of topics your application produces to</td><td><pre data-overflow="wrap"><code>SUPERSTREAM_TOPICS_LIST=orders,payments,user-events
</code></pre></td></tr><tr><td><code>SUPERSTREAM_LATENCY_SENSITIVE=false</code></td><td>No</td><td>Set to <code>true</code> to prevent any modification to linger.ms values</td><td><pre data-overflow="wrap"><code>SUPERSTREAM_LATENCY_SENSITIVE=true
</code></pre></td></tr><tr><td><code>SUPERSTREAM_DISABLED=false</code></td><td>No</td><td>Set to <code>true</code> to disable optimization</td><td><pre data-overflow="wrap"><code>SUPERSTREAM_DISABLED=true
</code></pre></td></tr></tbody></table>

#### Step 3: Instrument

Add Superstream Java agent to your application's startup command:

```bash
java -javaagent:/path/to/superstream-clients-1.0.0.jar -jar your-application.jar
```

Common example:

```bash
java -javaagent:$MAVEN_REPOSITORY$/ai/superstream/superstream-clients/1.0.0/superstream-clients-java-1.0.0.jar -jar your-application.jar
```

#### Docker Integration

When using Superstream Clients with containerized applications, include the agent in your Dockerfile:

```docker
FROM openjdk:11-jre

WORKDIR /app

# Copy your application
COPY target/your-application.jar app.jar

# Copy the Superstream agent
COPY path/to/superstream-clients-1.0.16.jar superstream-agent.jar

# Run with the Java agent
ENTRYPOINT ["java", "-javaagent:/app/superstream-agent.jar", "-jar", "/app/app.jar"]
```

Alternatively, you can use a multi-stage build to download the agent from Maven Central:

```docker
# Build stage
FROM maven:3.8-openjdk-11 AS build

# Get the Superstream agent
RUN mvn org.apache.maven.plugins:maven-dependency-plugin:3.2.0:get \
-DgroupId=ai.superstream \
-DartifactId=superstream-clients \
-Dversion=1.0.0

RUN mvn org.apache.maven.plugins:maven-dependency-plugin:3.2.0:copy \
-Dartifact=ai.superstream:superstream-clients:1.0.0 \
-DoutputDirectory=/tmp

# Final stage
FROM openjdk:11-jre

# Copy your application
COPY target/your-application.jar /app/your-application.jar

# Copy the agent from the build stage
COPY --from=build /tmp/superstream-clients-1.0.0.jar /app/lib/superstream-clients-1.0.0.jar
```

#### SUPERSTREAM\_LATENCY\_SENSITIVE Explained

The linger.ms parameter follows these rules:

1. If SUPERSTREAM\_LATENCY\_SENSITIVE is set to true:
   * Linger value will never be modified, regardless of other settings
2. If SUPERSTREAM\_LATENCY\_SENSITIVE is set to false or not set:
   * If no explicit linger exists in original configuration: Use Superstream's optimized value
   * If explicit linger exists: Use the maximum of original value and Superstream's optimized value

### Prerequisites

* Java 11 or higher
* Kafka cluster that is connected to the Superstream's console
* Read and write permissions to the `superstream.*` topics
