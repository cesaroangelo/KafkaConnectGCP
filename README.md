# Kafka Connect + Google Cloud Pub/Sub Connector

Kafka Connect with Google Cloud PubSub connector

	- https://kafka.apache.org/
	- https://github.com/GoogleCloudPlatform/pubsub/tree/master/kafka-connector

## Build

```shell
docker build -t artifactory.cesaro.io/kafka_connect_gcp .
```

### Configuration

This container builds the Apache Kafka Connect configuration in distribuited mode according to the value of the following env variables:

	- `KAFKACONNECT_BOOTSTRAP_SERVERS`: Comma-separated list of host and port pairs that are the addresses of the Kafka brokers. (Default: localhost:9092)
	- `KAFKACONNECT_GROUP_ID`: Unique string that identifies the Connect cluster group this worker belongs to. (Default: connect-cluster) 
	- `KAFKACONNECT_KEY_CONVERTER`: Converter class for key Connect data. (Default: org.apache.kafka.connect.json.JsonConverter)
	- `KAFKACONNECT_VALUE_CONVERTER`: Converter class for value Connect data. (Default: org.apache.kafka.connect.json.JsonConverter)
	- `KAFKACONNECT_KEY_CONVERTER_SCHEMAS_ENABLE`: (Default: true)
	- `KAFKACONNECT_VALUE_CONVERTER_SCHEMAS_ENABLE`: (Default: true)
	- `KAFKACONNECT_INTERNAL_KEY_CONVERTER`: (Default: org.apache.kafka.connect.json.JsonConverter)
	- `KAFKACONNECT_INTERNAL_VALUE_CONVERTER`: (Default: org.apache.kafka.connect.json.JsonConverter)
	- `KAFKACONNECT_INTERNAL_KEY_CONVERTER_SCHEMAS_ENABLE`: (Default: false)
	- `KAFKACONNECT_INTERNAL_VALUE_CONVERTER_SCHEMAS_ENABLE`: (Default: false)
        - `KAFKACONNECT_OFFSET_STORAGE_TOPIC`: (Default: connect-offsets)
	- `KAFKACONNECT_OFFSET_STORAGE_REPLICATION_FACTOR`: (Default: 1)
	- `KAFKACONNECT_CONFIG_STORAGE_TOPIC`: (Default: connect-configs)
	- `KAFKACONNECT_CONFIG_STORAGE_REPLICATION_FACTOR`: (Default: 1)
	- `KAFKACONNECT_STATUS_STORAGE_TOPIC`: (Default: connect-status)
	- `KAFKACONNECT_STATUS_STORAGE_REPLICATION_FACTOR`: (Default: 1)
	- `KAFKACONNECT_OFFSET_FLUSH_INTERVAL_MS`: (Default: 10000)
	- `KAFKACONNECT_REST_HOST_NAME`: (Default: localhost)
	- `KAFKACONNECT_REST_PORT`: (Default: 8083)

	- `KAFKACONNECT_LOG4J_ROOTLOGGER`: (Default: INFO, stdout, connectAppender)
	- `KAFKACONNECT_LOG4J_APPENDER_STDOUT`: (Default: org.apache.log4j.ConsoleAppender)
	- `KAFKACONNECT_LOG4J_APPENDER_STDOUT_LAYOUT`: (Default: org.apache.log4j.PatternLayout)
	- `KAFKACONNECT_LOG4J_APPENDER_STDOUT_LAYOUT_CONVERSATIONPATTERN`: (Default: [%d] %p %m (%c:%L)%n)
	- `KAFKACONNECT_LOG4J_LOGGER_ORG_APACHE_ZOOKEEPER`: (Default: ERROR)
	- `KAFKACONNECT_LOG4J_LOGGER_ORG_I0ITEC_ZKCLIENT`: (Default: ERROR)
	- `KAFKACONNECT_LOG4J_LOGGER_ORG_REFLECTIONS`: (Default: ERROR)
	- `KAFKACONNECT_LOG4J_APPENDER_CONNECTAPPENDER`: (Default: org.apache.log4j.DailyRollingFileAppender)
	- `KAFKACONNECT_LOG4J_APPENDER_CONNECTAPPENDER_DATEPATTERN`: (Default: '.'yyyy-MM-dd-HH)
	- `KAFKACONNECT_LOG4J_APPENDER_CONNECTAPPENDER_FILE`: (Default: ${kafka.logs.dir}/connect.log)
	- `KAFKACONNECT_LOG4J_APPENDER_CONNECTAPPENDER_LAYOUT`: (Default: org.apache.log4j.PatternLayout)
	- `KAFKACONNECT_LOG4J_APPENDER_CONNECTAPPENDER_LAYOUT_CONVERSIONPATTERN`: (Default: [%d] %p %m (%c:%L)%n)
	- `KAFKACONNECT_LOG4J_APPENDER_KAFKACONNECTRESTAPPENDER`: (Default: org.apache.log4j.DailyRollingFileAppender)
	- `KAFKACONNECT_LOG4J_APPENDER_KAFKACONNECTRESTAPPENDER_DATEPATTERN`: (Default: '.'yyyy-MM-dd-HH)
	- `KAFKACONNECT_LOG4J_APPENDER_KAFKACONNECTRESTAPPENDER_FILE`: (Default: ${kafka.logs.dir}/connect-rest.log)
        - `KAFKACONNECT_LOG4J_APPENDER_KAFKACONNECTRESTAPPENDER_LAYOUT`: (Default: org.apache.log4j.PatternLayout)
        - `KAFKACONNECT_LOG4J_APPENDER_KAFKACONNECTRESTAPPENDER_LAYOUT_CONVERSATIONPATTERN`: (Default: [%d] %p %m (%c)%n)
	- `KAFKACONNECT_LOG4J_LOGGER_ORG_APACHE_KAFKA_CONNECT_RUNTIME_REST`: (Default: INFO, kafkaConnectRestAppender)
	- `KAFKACONNECT_LOG4J_ADDITIVITY_ORG_APACHE_KAFKA_CONNECT_RUNTIME_REST`: (Default: false)

### Examples

```shell
	docker run --log-opt max-size=50m --log-driver=json-file -id --memory="2g" --memory-reservation="2g" --name kafka_connect_gcp_test --net=host \ 
	-p 8083:8083 \ 
	-v /data/kafkaConnect/json:/opt/kafka/json \ 
	-v /data/kafkaConnect/logs:/opt/kafka/logs \ 
	-e GOOGLE_APPLICATION_CREDENTIALS=/opt/kafka/json/credentials-test-pubsub.json \ 
	-e KAFKA_HEAP_OPTS="-Xmx1G -Xms1G" \ 
	-e KAFKACONNECT_BOOTSTRAP_SERVERS=<host>:9092,<host>:9092 \ 
	-e KAFKACONNECT_REST_HOST_NAME=<host> \ 
	-e KAFKACONNECT_REST_PORT=8083 \ 
	-e KAFKACONNECT_KEY_CONVERTER=org.apache.kafka.connect.storage.StringConverter \ 
	-e KAFKACONNECT_VALUE_CONVERTER=org.apache.kafka.connect.storage.StringConverter \ 
	-e KAFKACONNECT_OFFSET_STORAGE_REPLICATION_FACTOR=2 \ 
	-e http_proxy="<http://host:port>" \ 
	-e https_proxy="<http://host:port>" \ 
	-e KAFKA_OPTS="-Dhttp.proxyHost=<host> -Dhttp.proxyPort=<port> -Dhttps.proxyHost=<host> -Dhttps.proxyPort=<port>" \ 
	-e GRPC_PROXY_EXP="<host:port>" \ 
	-t artifactory.cesaro.io/kafka_connect_gcp:1.1
```

### Contents

The container holds the following services:
	- Apache Kafka Connect
	- Google Cloud PubSub Connector
