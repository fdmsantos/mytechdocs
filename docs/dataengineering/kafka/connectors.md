# Kafka Connectors

## Workers Configuration

```yaml
key.converter=org.apache.kafka.connect.storage.StringConverter
value.converter=io.confluent.connect.avro.AvroConverter
value.converter.schemas.enable=true
value.converter.schema.registry.url=http://localhost:8081
```

## HDFS Connector

```yaml
name=HdfsSinkConnector
connector.class=io.confluent.connect.hdfs.HdfsSinkConnector
flush.size=2
topics=tweets
tasks.max=1
hdfs.url=hdfs://172.17.0.1:9000/user/nobody/
```