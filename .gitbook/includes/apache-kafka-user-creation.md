---
title: Apache Kafka User Creation
---

```
// cluster ACLs
{"CLUSTER", "kafka-cluster", "LITERAL", "ALTER_CONFIGS", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"}
{"CLUSTER", "kafka-cluster", "LITERAL", "CREATE", "ALLOW"}

// consumers groups ACLs
{"GROUP", "*", "LITERAL", "DELETE", "ALLOW"}
{"GROUP", "*", "LITERAL", "DESCRIBE", "ALLOW"}
{"GROUP", "*", "LITERAL", "READ", "ALLOW"}

// topics ACLs
{"TOPIC", "*", "LITERAL", "ALTER", "ALLOW"}
{"TOPIC", "*", "LITERAL", "ALTER_CONFIGS", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DELETE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DESCRIBE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "DESCRIBE_CONFIGS", "ALLOW"}
{"TOPIC", "*", "LITERAL", "READ", "ALLOW"}
{"TOPIC", "*", "LITERAL", "WRITE", "ALLOW"}
{"TOPIC", "*", "LITERAL", "CREATE", "ALLOW"}
```

###

The following information will be required for each cluster:

* Bootstrap servers (Kafka URL)
* Authentication security protocol (No auth / SSL / SASL\_SSL)
  * SSL with validation "on" would require a `key.pem`,`cert.pem`, and `ca.pem`&#x20;
* JMX port (and token if needed)
