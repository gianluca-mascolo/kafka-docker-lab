# Kafka on docker

## build docker images
docker-compose build

## create kafka stack
docker-compose up -d

## create topic
docker exec -ti broker kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1  --partitions 1 --topic prova
docker exec -ti broker kafka-topics.sh --zookeeper zookeeper:2181 --describe --topic test

## produce to topic
docker exec -ti broker kafka-console-producer.sh --broker-list localhost:9092 --topic prova

## consume from topic
docker exec -ti broker kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic prova --from-beginning
