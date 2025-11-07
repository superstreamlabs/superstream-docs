---
description: This page overviews the Superstream required networking
---

# Firewall rules

### Firewall rules

<table data-header-hidden data-full-width="true"><thead><tr><th>Source service (Pod)</th><th>Destination</th><th>Port</th><th>UDP/TCP</th><th>Internal VPC / External VPC</th><th>Destination addresses</th><th>Type of data</th><th>Throughput</th></tr></thead><tbody><tr><td>All pods</td><td>Telegraf</td><td>6514</td><td>UDP</td><td>Internal</td><td></td><td>Logs</td><td>1-5 MBs/day</td></tr><tr><td>Telegraf</td><td>All pods</td><td>7777</td><td>TCP</td><td>Internal</td><td></td><td>Logs</td><td>1-5 MBs/day</td></tr><tr><td>Agent</td><td>Kafka</td><td>Kafka Port</td><td>TCP</td><td>Internal</td><td>Kafka boostrap urls</td><td>Metadata such as topic names, consumer groups, configuration</td><td>10-50 MBs/day</td></tr><tr><td>Datadog</td><td>Kafka</td><td>Kafka JMX     Port</td><td>TCP</td><td>External</td><td><a href="https://process.us5.datadoghq.com">https://*.us5.datadoghq.com</a></td><td>Metrics</td><td>100-150 MBs/day</td></tr><tr><td>Telegraf</td><td>Superstream Platform</td><td>443</td><td>HTTPS</td><td>External</td><td><a href="https://loki.mgmt.superstream.ai/">https://loki.mgmt.superstream.ai</a><a href="https://prometheus.mgmt.superstream.ai">https://prometheus.mgmt.superstream.ai</a></td><td>Logs</td><td>1-5 MBs/day</td></tr><tr><td>Agent</td><td>Superstream Platform</td><td>9440</td><td>TCP</td><td>External</td><td>hr72spwylm.us-east-1.aws.clickhouse.cloud<br></td><td>Metadata such as topic names, consumer groups, configuration</td><td>1-5 MBs/day</td></tr><tr><td>Agent</td><td>Superstream Platform</td><td>4222<br></td><td>TCP</td><td>External</td><td>broker.superstream.ai</td><td>Client commands, remediations</td><td>&#x3C; 1 MBs/day</td></tr><tr><td>Agent</td><td>AWS / Aiven / Confluent API</td><td>443</td><td>TCP</td><td>External</td><td>ANY</td><td>Metrics, Billing</td><td>&#x3C; 1 MBs/day</td></tr></tbody></table>

