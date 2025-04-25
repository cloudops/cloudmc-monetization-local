# cloudmc-monetization-local
Docker compose file to setup local ES and Kafka environment.

## Requirements

- Docker host with at least 6 GB of memory
- `docker-compose`

## Installation

To install and run ES and Kafka:

```
docker-compose up -d
```

NB: if the containers exit with code 137, this means they are OOM. Add more memory

## Kibana
If you would like to also have kibana locally (allowing you to visually explore your records, index management, etc), you can uncomment the commented-out sections of the docker-compose.yaml and access the app at `localhost:5601/app`.

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
cloudmc.kafka.security.protocol=PLAINTEXT
cloudmc.kafka.topic.replicas=1

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

## Connection problem in AKHQ
Depending on your version of docker, you may be experiencing some connectivity issues in AKHQ to Kafka.
Please verify first that both Zookeeper and Kafka containers are up. If it is so, you need to do an update to the docker-compose.yaml file. 

You must update the file from : 
```
  akhq:
    environment:
      AKHQ_CONFIGURATION: |
        akhq:
          connections:
            docker-kafka-server:
              properties:
                bootstrap.servers: "cloudmc-monetization-local-kafka-1:9092"
```
to :
```
  akhq:
    environment:
      AKHQ_CONFIGURATION: |
        akhq:
          connections:
            docker-kafka-server:
              properties:
                bootstrap.servers: "cloudmc-monetization-local_kafka_1:9092"
```

Then run again the docker-compose command.
```
docker-compose up -d
```


It seems the docker container name generation changed in the most recent version of docker. They now use hyphens instead of underscores.