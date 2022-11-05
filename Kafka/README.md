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

### Consumir mensagens de um determinado tópico (especificando o Group ID)
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NOME_DO_TOPICO --consumer-property group.id=nome-do-group-id

### Descrever os conteúdos dos tópicos
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic NOME_DO_TOPICO

### Alterar número de partições de um tópico
    bin/kafka-topics.sh --alter --zookeeper localhost:2181 --topic NOME_DO_TOPICO --partitions 2

### Descrever os consumer groups
    bin/kafka-consumer-groups.sh --all-groups --bootstrap-server localhost:9092 --describe

### Replicação em cluster
Necessário criar um arquivo de configuração config/server.properties para cada réplica (cluster).

Exemplo: Réplica 1 = server.properties / Réplica 2 - server2.properties / Réplica 3 - server3.properties

&nbsp;

Parâmetros que devem ser ajustados em cada arquivo de configuração para cada réplica:

The id of the broker. This must be set to a unique integer for each broker.

Colocar 1 ID único para cada réplica.
    
    broker.id=0

&nbsp;

A comma separated list of directories under which to store log files

Setar o diretório de log diferente para cada réplica.

    log.dirs=/tmp/kafka-logs

&nbsp;

Setar uma porta diferente para cada répica.

    listeners=PLAINTEXT://:9092

&nbsp;

Ajustar os parâmetros do Internal Topic Settings:

    ############################# Internal Topic Settings  #############################
    # The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
    # For anything other than development testing, a value greater than 1 is recommended for to ensure availability such as 3.
    offsets.topic.replication.factor=3
    transaction.state.log.replication.factor=3
    transaction.state.log.min.isr=1

&nbsp;

Inserir também o seguinte parâmetro de acordo com o número de réplicas que desejar:

    ############################# Replication Factor Config #############################
    default.replication.factor = 3

&nbsp;

Obs.: Os tópicos devem ser recriados após a configuração de replicação. Para recriar os tópicos, apagar os dados da pasta configurada no parâmetro log.dirs.