# Java

### General guidelines

* Superstream dependency is a replacement for `kafka-clients` dependency.
* No other `kafka-clients` dependency should run concurrently with the Superstem dependency. The Superstem dependency must be the sole `kafka-clients` dependency used across different packages in the same pom.xml file.

### Replacement example

* Let's assume that your application is using both `spring-kafka` and `kafka-streams` packages.
* Your pom.xml should look similar to this:

{% code lineNumbers="true" %}
```java
<dependencies>
    <dependency>
        <groupId>org.springframework.kafka</groupId>
        <artifactId>spring-kafka</artifactId>
        <version>2.8.4</version>
    </dependency>

    <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-streams</artifactId>
        <version>3.5.1</version> <!-- Replace with the desired version -->
    </dependency>
</dependencies>
```
{% endcode %}

* Both packages are using `kafka-clients` as an internal dependency. We need to exclude it.

{% code lineNumbers="true" %}
```java
<dependencies>
    <dependency>
        <groupId>org.springframework.kafka</groupId>
        <artifactId>spring-kafka</artifactId>
        <version>2.8.4</version>
        <exclusions>
            <exclusion>
                <groupId>org.apache.kafka</groupId>
                <artifactId>kafka-clients</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-streams</artifactId>
        <version>3.5.1</version> <!-- Replace with the desired version -->
        <exclusions>
            <exclusion>
                <groupId>org.apache.kafka</groupId>
                <artifactId>kafka-clients</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```
{% endcode %}

* Now, you just need to add the right Superstream dependency based on the version you are currently using

{% code lineNumbers="true" %}
```java
<dependencies>
    <dependency>
        <groupId>org.springframework.kafka</groupId>
        <artifactId>spring-kafka</artifactId>
        <version>2.8.4</version>
        <exclusions>
            <exclusion>
                <groupId>org.apache.kafka</groupId>
                <artifactId>kafka-clients</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-streams</artifactId>
        <version>3.5.1</version> <!-- Replace with the desired version -->
        <exclusions>
            <exclusion>
                <groupId>org.apache.kafka</groupId>
                <artifactId>kafka-clients</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    
    <dependency>
        <groupId>ai.superstream</groupId>
        <artifactId>kafka-clients</artifactId>
        <version>3.5.14</version><!-- Replace with the desired version -->
    </dependency>
</dependencies>
```
{% endcode %}

#### A list of all the known packages that are using `kafka-clients` and requires exclusion

* Kafka Streams
* Kafka Connect
* Confluent Schema Registry Client
* Confluent Kafka REST
* Confluent Kafka Avro Serializer/Deserializer
* Spring Kafka
* Apache Flink Kafka Connector
* Apache Camel Kafka Component
* Akka Streams Kafka
* Kafka Clients
