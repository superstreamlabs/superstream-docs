---
description: This page describes how to connect your Kafka clients to Superstream
---

# Step 4: Connect Your Clients

Superstream client is a supplement for your existing Kafka library.

## Key features

* Identical to your existing Kafka SDKs
* Easy to replace
* You control your Kafka Clients: batch, linger, compression, data format
* Enforce governance
* Runtime client configuration updates
* Transparent client migration between topics and clusters
* Zero additional computing resources are needed

## Getting started

### Step 0: Confirm that the `superstream.metadata` topic has been created

As described in the [prerequisites](../step-1-prerequisites.md#id-1.-a-user-for-superstream-per-kafka-cluster) page, a topic named `superstream.metadata` has to be created with the following permissions: Read, Create, and Write.

### Step 1: Set environment variable

Please set the following environment variables for each connected client

{% code title="" lineNumbers="true" %}
```bash
#Required. Superstream local engine URL to connect the client to.
SUPERSTREAM_HOST="superstream-nats.orgname.com"

#Optional. Enable compression as soon as the client starts
SUPERSTREAM_REDUCTION_ENABLED=false

#Optional. Adding tags will help you to identify this client, attach configuration, send emails, and more.
#There are no specific keys to use; add as you want and as many as needed.
SUPERSTREAM_TAGS={"owner":'Johnd@google.com', "config":'prod'}

#Optional. Debug mode for additional client logs
SUPERSTREAM_DEBUG=false
```
{% endcode %}

### Step 2: Library replacement

Within each lib repo made by Superstream, under the "Releases" section, you will find the equivalent version to the existing "stable" library according to the major release.

For example, if you are currently using Apache Kafka 3.1.0, then you would use the latest release of 3.x.x by Superstream.

| Language        | Source SDK                                                                                                | Superstream equivalent                                                                            |
| --------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| [Java](java.md) | [Native Apache Kafka Java](https://central.sonatype.com/artifact/org.apache.kafka/kafka-clients/overview) | [Native Apache Kafka Java](https://github.com/superstreamlabs/apache-kafka-java-sdk/releases)     |
|                 | [Cloudera 2.8.x](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients?repo=cloudera-repos)   | [Native Apache Kafka Java](https://github.com/superstreamlabs/apache-kafka-java-sdk/releases)     |
|                 | [spring-kafka](https://central.sonatype.com/artifact/org.springframework.kafka/spring-kafka)              | [Native Apache Kafka Java](https://github.com/superstreamlabs/apache-kafka-java-sdk/releases)     |
| Python          | [confluent-python](https://github.com/confluentinc/confluent-kafka-python)                                | [superstream-confluent-kafka](https://github.com/superstreamlabs/confluent-kafka-python/releases) |



## Appendixes

### Appendix A - Securing Client Connections

To enable secured client connections to the Superstream Engine, ensure that `skipLocalAuthentication` is turned off. Detailed instructions can be found [here](https://docs.superstream.ai/getting-started/step-2-engine-deployment#appendix-f-deploy-superstream-engine-with-internal-authentication-mode-on).

#### For clients with enabled authentication, set the following environment variable for each client in addition:

{% code lineNumbers="true" %}
```bash
SUPERSTREAM_TOKEN=
## This token is required to access Superstream engine securely.
```
{% endcode %}

### Appendix B - Setting Up a Maven Repository and Managing Artifacts with JFrog Artifactory

#### **Prerequisites**

1. Access to a JFrog Artifactory instance.
2. GitHub account and access to a repository with releases.

#### **Create a New Maven Repository**

1. Log in to your JFrog Artifactory.
2. Navigate to Admin > Repositories > Repositories.
3. Click on 'Add Repositories' and select 'Local Repository'.
4. Choose 'Maven' as the package type.
5. Enter the repository key and other configuration details.
6. Save the repository settings.

#### **Download the artifact from GitHub:**&#x20;

[https://github.com/superstreamlabs/apache-kafka-java-sdk/releases](https://github.com/superstreamlabs/apache-kafka-java-sdk/releases)​

#### **Find your** equivalent **release**

1. Navigate to the 'Releases' section of the desired GitHub repository.
2. Select the release that contains the needed `kafka-clients-yourversion.tar.gz` file.

**Download the artifact**

1. Right-click on the `kafka-clients-yourversion.tar.gz` link and copy the URL.
2. Use `curl` or `wget` to download the file from the terminal.

**Deploy via the JFrog UI**

1. Navigate to the Artifactory repository where you want to deploy the artifact.
2. Click on 'Deploy' and select the `kafka-clients-yourversion.tar.gz` file from your local system.
3. Specify the target path within the repository if required.
4. Click 'Deploy' to upload the artifact.

## Knowledge Base

**Q:** I am using Kafka Connect. How can I ensure Superstream identifies it when declaring a topic inactive?

**A:** To enable Superstream to detect a source or sink that is activated for a specific period and then removed, the connector must be configured with a <mark style="color:purple;">`group.id`</mark>. This configuration will "leave a trace" in the form of a zombie consumer group. **You can find an example of this from Confluent** [**here**](https://docs.confluent.io/platform/current/installation/configuration/connect/index.html#group-id).
