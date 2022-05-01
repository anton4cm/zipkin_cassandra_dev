```sh
# CREATE NETWORK
docker network create salesforce-spring-network

# TEST CONFIG
cd /DATA/TRACING_NEW/
docker-compose --compatibility -p salesforce_tracing_new config

# STOP AND REMOVE OLD CONTAINERS
cd /DATA/TRACING_NEW/
docker-compose --compatibility -p salesforce_tracing_new rm -fs zipkin-service
docker-compose --compatibility -p salesforce_tracing_new rm -fs cassandra-service
docker-compose --compatibility -p salesforce_tracing_new rm -fs rabbitmq-service

# UP TRACING
cd /DATA/TRACING_NEW/
docker-compose --compatibility -p salesforce_tracing_new up -d rabbitmq-service
docker logs -f rabbitmq-service-new
#docker-compose --compatibility -p salesforce_tracing_new exec rabbitmq-service rabbitmqctl set_vm_memory_high_watermark 0.7

# UP CASSANDRA
cd /DATA/TRACING_NEW/
docker-compose --compatibility -p salesforce_tracing_new up -d cassandra-service
docker logs -f cassandra-service-new

# UP ZIPKIN
cd /DATA/TRACING_NEW/
docker-compose --compatibility -p salesforce_tracing_new up -d zipkin-service
docker logs -f zipkin-service-new

# LOGS
docker logs -f --tail 100 rabbitmq-service-new
docker logs -f --tail 100 cassandra-service-new
docker logs -f --tail 100 zipkin-service-new

# VIEW ALL LOGS
cd /DATA/TRACING_NEW/
docker-compose --compatibility -p salesforce_tracing_new logs -f --tail 1

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
cd /DATA/TRACING_NEW/

# Check config and status
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new config
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new ps
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new top

# Destroy all
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new down

# Destroy per service
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new rm -fs zipkin-service
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new rm -fs cassandra-service
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new rm -fs rabbitmq-service

# Remove volumes
docker volume rm salesforce_tracing_new_cassandra_vol_new
docker volume rm salesforce_tracing_new_rabbitmq_vol_new

# Up
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new up -d --build rabbitmq-service
docker logs -f rabbitmq-service-new

docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new up -d --build cassandra-service
docker logs -f cassandra-service-new

docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new up -d --build zipkin-service
docker logs -f zipkin-service-new

# Logs
docker logs -f --tail 100 rabbitmq-service-new
docker logs -f --tail 100 cassandra-service-new
docker logs -f --tail 100 zipkin-service-new
docker-compose --compatibility -f /DATA/TRACING_NEW/docker-compose.yml -p salesforce_tracing_new logs -f --tail 1
```

## RABBIT [usuario/password: guest/guest] (19672)
* http://192.168.9.197:19672/ (DESARROLLO)
* http://IP_CALIDAD:19672/ (CALIDAD)
* http://IP_PRODUCCION:19672/  (PRODUCCION)

## ZIPKIN (9911)
* http://192.168.9.197:9911/ (DESARROLLO)
* http://IP_CALIDAD:9911/ (CALIDAD)
* http://IP_PRODUCCION:9911/  (PRODUCCION)
