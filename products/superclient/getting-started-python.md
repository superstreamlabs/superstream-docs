# Getting started - Python

Apache Kafka is a high-throughput distributed messaging system that powers many real-time applications. However, Kafka's performance can often be constrained by inefficient network usage—especially in high-throughput or multi-region deployments. Improving Kafka’s network efficiency means optimizing how data flows between clients and brokers, reducing bandwidth usage, minimizing latency, and ultimately ensuring cost-effective and reliable data pipelines.

At Superstream, we can make it easier to manage and optimize Kafka networking, particularly through our open-source `superstream-clients` library. This guide walks through how to use the library to boost network efficiency when interacting with Kafka.

***

## Superstream Client For Python

A Python library for automatically optimizing Kafka producer configurations based on topic-specific recommendations.

### Overview

Superstream Clients works as a Python import hook that intercepts Kafka producer creation and applies optimized configurations without requiring any code changes in your application. It dynamically retrieves optimization recommendations from Superstream and applies them based on impact analysis.

### Supported Libraries

Works with any Java library that depends on `kafka-clients`, including:

* kafka-python
* aiokafka
* confluent-kafka
* Faust
* FastAPI event publishers
* Celery Kafka backends
* Any custom wrapper around these Kafka clients

### Features

* **Zero-code integration**: No code changes required in your application
* **Dynamic configuration**: Applies optimized settings based on topic-specific recommendations
* **Intelligent optimization**: Identifies the most impactful topics to optimize
* **Graceful fallback**: Falls back to default settings if optimization fails

***

### Installation

_Superstream package_: [https://pypi.org/project/superstream-clients](https://pypi.org/project/superstream-clients/)

#### Step 0: Add permissions

Any app that runs Superstream lib should be able to `READ/WRITE/DESCRIBE` from all topics with the prefix `superstream.*`

#### Step 1: Install the Superstream lib

```bash
pip install superstream-clients && python -m superclient install_pth
```

#### Step 2: Add Environment Variables

<table data-full-width="false"><thead><tr><th width="341.51953125">ENV</th><th width="107.9453125">Required?</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>SUPERSTREAM_TOPICS_LIST</code></td><td>Yes</td><td>Comma-separated list of topics your application produces to</td><td><pre data-overflow="wrap"><code>SUPERSTREAM_TOPICS_LIST=orders,payments,user-events
</code></pre></td></tr><tr><td><code>SUPERSTREAM_LATENCY_SENSITIVE=false</code></td><td>No</td><td>Set to <code>true</code> to prevent any modification to linger.ms values</td><td><pre data-overflow="wrap"><code>SUPERSTREAM_LATENCY_SENSITIVE=true
</code></pre></td></tr><tr><td><code>SUPERSTREAM_DISABLED=false</code></td><td>No</td><td>Set to <code>true</code> to disable optimization</td><td><pre data-overflow="wrap"><code>SUPERSTREAM_DISABLED=true
</code></pre></td></tr></tbody></table>



That's it! Superclient will now automatically load and optimize all Kafka producers in your Python environment.

After installation, SuperClient works automatically. Just use your Kafka clients as usual.

#### Optional. Docker Integration

When using Superstream Clients with containerized applications, include the package in your Dockerfile:

```docker
FROM python:3.8-slim

# Install superclient
RUN pip install superstream-clients
RUN python -m superclient install_pth

# Your application code
COPY . /app
WORKDIR /app

# Run your application
CMD ["python", "your_app.py"]
```

### Prerequisites

* Python 3.8 or higher
* Kafka cluster that is connected to the Superstream's console
* Read and write permissions to the `superstream.*` topics
