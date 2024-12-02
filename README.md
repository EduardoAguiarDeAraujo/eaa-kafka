# Apache Kafka

## Configurando Container Docker

### Criar arquivo docker-compose.yml:

```yml
version: '3.8'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - "2181:2181"

  kafka:
    image: confluentinc/cp-kafka:latest
    container_name: kafka
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```
#### Subir os containers:

```bash
docker-compose up -d
```

#### Verificar os containers:

```bash
docker ps
```

### Testar a Configuração:

#### Acessar o container Kafka:
```bash
docker exec -it kafka bash
```

#### Criar um tópico: 
```bash
kafka-topics --create --topic meu-topico --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

#### Listar os tópicos:
```bash
kafka-topics --list --bootstrap-server localhost:9092
```


#### Enviar mensagens (Producer):
```bash
kafka-console-producer --topic meu-topico --bootstrap-server localhost:9092
```

#### Ler mensagens (Consumer):
```bash
kafka-console-consumer --topic meu-topico --bootstrap-server localhost:9092 --from-beginning
```

