```sh
# CREATE NETWORK
docker network create salesforce-spring-network

# TEST CONFIG
cd /DATA/TRACING/
docker-compose --compatibility config

# STOP AND REMOVE OLD CONTAINERS
cd /DATA/TRACING/
docker-compose --compatibility rm -fs zipkin-service
docker-compose --compatibility rm -fs cassandra-service
docker-compose --compatibility rm -fs rabbitmq-service

# UP TRACING
cd /DATA/TRACING/
docker-compose --compatibility up -d rabbitmq-service
docker logs -f rabbitmq-service
#docker-compose --compatibility exec rabbitmq-service rabbitmqctl set_vm_memory_high_watermark 0.7

# UP CASSANDRA
cd /DATA/TRACING/
docker-compose --compatibility up -d cassandra-service
docker logs -f cassandra-service

# UP ZIPKIN
cd /DATA/TRACING/
docker-compose --compatibility up -d zipkin-service
docker logs -f zipkin-service

# LOGS
docker logs -f --tail 100 rabbitmq-service
docker logs -f --tail 100 cassandra-service
docker logs -f --tail 100 zipkin-service

# VIEW ALL LOGS
cd /DATA/TRACING/
docker-compose --compatibility logs -f --tail 1

```


# Docker version 19.03.12, build 48a66213fe
```
docker --version
```

# docker-compose version 1.25.4, build 8d51620a
```
docker-compose --version
```

# Verificar la existencia del network 'salesforce-spring-network', caso contrario crearla
```
docker network ls
docker network create salesforce-spring-network
```

## TRACING
```sh
cd /DATA/TRACING/

# Check config and status
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml config
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml ps
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml top

# Destroy all
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml down

# Destroy per service
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml rm -fs zipkin-service
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml rm -fs cassandra-service
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml rm -fs rabbitmq-service

# Remove volumes
docker volume rm tracing_cassandra-vol
docker volume rm tracing_rabbitmq-vol

# Up
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml up -d --build rabbitmq-service
docker logs -f rabbitmq-service

docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml up -d --build cassandra-service
docker logs -f cassandra-service

docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml up -d --build zipkin-service
docker logs -f zipkin-service

# Logs
docker logs -f --tail 100 rabbitmq-service
docker logs -f --tail 100 cassandra-service
docker logs -f --tail 100 zipkin-service
docker-compose --compatibility -f /DATA/TRACING/docker-compose.yml logs -f --tail 1
```

## RABBIT [usuario/password: guest/guest] (19672)
* http://192.168.9.197:19672/ (DESARROLLO)
* http://IP_CALIDAD:19672/ (CALIDAD)
* http://IP_PRODUCCION:19672/  (PRODUCCION)

## ZIPKIN (9911)
* http://192.168.9.197:9911/ (DESARROLLO)
* http://IP_CALIDAD:9911/ (CALIDAD)
* http://IP_PRODUCCION:9911/  (PRODUCCION)
