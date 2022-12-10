# PostgreSQL Debezium Example

This demo automatically deploys the topology of services as defined in the [Debezium Tutorial](https://debezium.io/documentation/reference/stable/tutorial.html).

- [PostgreSQL Debezium Example](#postgresql-debezium-example)
  - [Environment](#environment)
  - [Start](#start)
  - [Debugging](#debugging)

## Environment

| name           | version                |
| -------------- | ---------------------- |
| Docker Compose | 3.9                    |
| PostgreSQL     | postgres:13.9          |
| kafka          | debezium/kafka:2.0     |
| zookeeper      | debezium/zookeeper:2.0 |
| debezium       | debezium/connect:2.0   |

## Start

```shell
# Start the topology as defined in https://debezium.io/documentation/reference/stable/tutorial.html
docker-compose up

# Start Postgres connector
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-postgres.json

# Modify records in the database via Postgres client
docker-compose exec postgres env PGOPTIONS="--search_path=inventory" bash -c 'psql -U $POSTGRES_USER postgres'

# Shut down the cluster
docker-compose down
```

## Debugging

Should you need to establish a remote debugging session into a deployed connector, add the following to the `environment` section of the `connect` in the Compose file service:

    - KAFKA_DEBUG=true
    - DEBUG_SUSPEND_FLAG=n
    - JAVA_DEBUG_PORT=*:5005

Also expose the debugging port 5005 under `ports`:

    - 5005:5005

You can then establish a remote debugging session from your IDE on localhost:5005.
