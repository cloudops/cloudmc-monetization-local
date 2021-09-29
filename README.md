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