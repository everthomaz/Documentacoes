# Comandos do KAFKA

[Clique aqui para baixar o Kafka](https://kafka.apache.org/downloads)

### Iniciar o Zookeeper
    bin/zookeeper-server-start.sh config/zookeeper.properties

### Iniciar o Kafka (bootstrap-server)
    bin/kafka-server-start.sh config/server.properties

### Criar um t�pico
    bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic NOME_DO_TOPICO

### Listar t�picos
    bin/kafka-topics.sh --list --bootstrap-server localhost:9092

### Produzir mensagens para um determinado t�pico
    bin/kafka-console-producer.sh --broker-list localhost:9092 --topic NOME_DO_TOPICO
(Cada linha que digitar � uma mensagem).

### Consumir mensagens de um determinado t�pico (deste momento em diante)
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NOME_DO_TOPICO

### Consumir mensagens de um determinado t�pico (desde o in�cio, todas as mensagens)
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NOME_DO_TOPICO --from-beginning

### Consumir mensagens de um determinado t�pico (especificando o Group ID)
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic NOME_DO_TOPICO --consumer-property group.id=nome-do-group-id

### Descrever os conte�dos dos t�picos
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic NOME_DO_TOPICO

### Alterar n�mero de parti��es de um t�pico
    bin/kafka-topics.sh --alter --zookeeper localhost:2181 --topic NOME_DO_TOPICO --partitions 2

### Descrever os consumer groups
    bin/kafka-consumer-groups.sh --all-groups --bootstrap-server localhost:9092 --describe

### Replica��o em cluster
Necess�rio criar um arquivo de configura��o config/server.properties para cada r�plica (cluster).

Exemplo: R�plica 1 = server.properties / R�plica 2 - server2.properties / R�plica 3 - server3.properties

&nbsp;

Par�metros que devem ser ajustados em cada arquivo de configura��o para cada r�plica:

The id of the broker. This must be set to a unique integer for each broker.

Colocar 1 ID �nico para cada r�plica.
    
    broker.id=0

&nbsp;

A comma separated list of directories under which to store log files

Setar o diret�rio de log diferente para cada r�plica.

    log.dirs=/tmp/kafka-logs

&nbsp;

Setar uma porta diferente para cada r�pica.

    listeners=PLAINTEXT://:9092

&nbsp;

Ajustar os par�metros do Internal Topic Settings:

    ############################# Internal Topic Settings  #############################
    # The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
    # For anything other than development testing, a value greater than 1 is recommended for to ensure availability such as 3.
    offsets.topic.replication.factor=3
    transaction.state.log.replication.factor=3
    transaction.state.log.min.isr=1

&nbsp;

Inserir tamb�m o seguinte par�metro de acordo com o n�mero de r�plicas que desejar:

    ############################# Replication Factor Config #############################
    default.replication.factor = 3

&nbsp;

Obs.: Os t�picos devem ser recriados ap�s a configura��o de replica��o. Para recriar os t�picos, apagar os dados da pasta configurada no par�metro log.dirs.