# sentimental-birds

Sentimental analysis and visualization on Twitter streams.

# prerequisites

## kafka-connect-twitter plugin: https://github.com/jcustenborder/kafka-connect-twitter

```
cd kafka-connect-twitter
mvn clean package
cd target
tar -xvf kafka-connect-twitter-0.2-SNAPSHOT.tar.gz
```

## confluent platform docker images: https://github.com/confluentinc/cp-docker-images

## Change docker compose file in /examples/cp-all-in-one:
* Add plugins path: CONNECT_PLUGIN_PATH: '/usr/share/java'
* Add volume with twitter plugin

```
  connect:
    image: confluentinc/cp-kafka-connect
    hostname: connect
    depends_on:
      - zookeeper
      - broker
      - schema_registry
    ports:
      - "8083:8083"
    environment:
      CONNECT_BOOTSTRAP_SERVERS: 'broker:9092'
      CONNECT_REST_ADVERTISED_HOST_NAME: connect
      CONNECT_REST_PORT: 8083
      CONNECT_GROUP_ID: compose-connect-group
      CONNECT_CONFIG_STORAGE_TOPIC: docker-connect-configs
      CONNECT_CONFIG_STORAGE_REPLICATION_FACTOR: 1
      CONNECT_OFFSET_FLUSH_INTERVAL_MS: 10000
      CONNECT_OFFSET_STORAGE_TOPIC: docker-connect-offsets
      CONNECT_OFFSET_STORAGE_REPLICATION_FACTOR: 1
      CONNECT_STATUS_STORAGE_TOPIC: docker-connect-status
      CONNECT_STATUS_STORAGE_REPLICATION_FACTOR: 1
      CONNECT_KEY_CONVERTER: io.confluent.connect.avro.AvroConverter
      CONNECT_KEY_CONVERTER_SCHEMA_REGISTRY_URL: 'http://schema_registry:8081'
      CONNECT_VALUE_CONVERTER: io.confluent.connect.avro.AvroConverter
      CONNECT_VALUE_CONVERTER_SCHEMA_REGISTRY_URL: 'http://schema_registry:8081'
      CONNECT_INTERNAL_KEY_CONVERTER: org.apache.kafka.connect.json.JsonConverter
      CONNECT_INTERNAL_VALUE_CONVERTER: org.apache.kafka.connect.json.JsonConverter
      CONNECT_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      CONNECT_PLUGIN_PATH: '/usr/share/java'
    volumes:
      - LOCAL_KAFKA_CONNECT_TWITTER_PLUGIN_PATH:/usr/share/java/kafka-connect-twitter
```

# docker 

Deploy and start kafka connect twitter plugin:

```
curl -X POST -H "Content-Type: application/json" localhost:8083/connectors --data-binary @twitter-connector.json
```
