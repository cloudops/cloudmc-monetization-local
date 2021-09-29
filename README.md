# cloudmc-monetization-local
Docker compose file to setup local ES and Kafka environment.

## Requirements

- Docker host with at least 6 GB of memory

## cloudmc.properties

To connect to these dependencies, update your local cloudmc.properties to set:

```
# Elasticsearch rest client default properties
cloudmc.elasticsearch.rest.username=elastic
cloudmc.elasticsearch.rest.password=changeme
cloudmc.elasticsearch.rest.uris=localhost:9200
cloudmc.elasticsearch.rest.connection-timeout=1000
cloudmc.elasticsearch.rest.read-timeout=30000

# Usage records pushed to kafka topic
cloudmc.kafka.bootstrap.server=localhost:29092
```

## Manually sending data to a topic

To manually send data to the topic, run this command:

`docker run -it --network=host edenhill/kcat:1.7.0 -b localhost:29092 -t YOUR_TOPIC -P `

It enters an interactive mode and each new line will be a distinct message. To exit, press `Ctrl+D`

More info [here](https://docs.confluent.io/platform/current/app-development/kafkacat-usage.html#producer-mode)

## Manually reading data from a topic

To manually read data from a topic, run this command:

`docker run -it --network=host edenhill/kcat:1.7.0 -b localhost:29092 -t YOUR_TOPIC`

To exit, press `Ctrl+C`

More info [here](https://docs.confluent.io/platform/current/app-development/kafkacat-usage.html#consumer-mode)