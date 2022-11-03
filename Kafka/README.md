# Comandos do KAFKA

[Clique aqui para baixar o Kafka](https://kafka.apache.org/downloads)

### Iniciar o Zookeeper
    bin/zookeeper-server-start.sh config/zookeeper.properties

### Iniciar o Kafka (bootstrap-server)
    bin/kafka-server-start.sh config/server.properties

### Criar um tópico
    bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic NOME_DO_TOPICO

### Listar tópicos
    bin/kafka-topics.sh --list --bootstrap-server localhost:9092

### Produzir mensagens para um determinado tópico
    bin/kafka-console-producer.sh --broker-list localhost:9092 --topic NOME_DO_TOPICO
(Cada linha que digitar é uma mensagem).

### Consumir mensagens de um determinado tópico (deste momento em diante)
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NOME_DO_TOPICO

### Consumir mensagens de um determinado tópico (desde o início, todas as mensagens)
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NOME_DO_TOPICO --from-beginning

### Descrever os conteúdos dos tópicos
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic NOME_DO_TOPICO

### Alterar número de partições de um tópico
    bin/kafka-topics.sh --alter --zookeeper localhost:2181 --topic NOME_DO_TOPICO --partitions 2

### Descrever os consumer groups
    bin/kafka-consumer-groups.sh --all-groups --bootstrap-server localhost:9092 --describe