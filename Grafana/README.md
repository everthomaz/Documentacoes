# Informa��es ref. ao GRAFANA

Prepara��o do ambiente:
- VirtualBox - Vers�o: 6.1.34
- Vagrant [Clique aqui para baixar o Vagrant](https://www.vagrantup.com/)
- Cmder [Clique aqui para baixar o Cmder](https://cmder.net/)

&nbsp;

### Neste passo-a-passo, ser� visto como instalar o Grafana utilizando uma m�quina virtual atrav�s do Vagrant.

&nbsp;

##### Subindo a m�quina virtual:

Colocar o arquivo Vagrantfile (que se encontra neste projeto dentro da pasta Arquivos) dentro de uma pasta e dentro da pasta executar:
    
    vagrant up

##### Acessar a m�quina virtual:
    vagrant ssh

##### Atualizar os pacotes do Linux.
    sudo apt-get update && sudo apt-get upgrade -y

##### Baixar o script oficial do Docker para instalar um container do Grafana.
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh

##### Adicionar o usu�rio vagrant no grupo do docker:
    sudo usermod -aG docker vagrant

##### Ap�s este comando, sair da m�quina e logar novamente:
    exit
    vagrant ssh

##### Testando a instala��o:
    docker ps

##### Criar pasta para o volume para persistir os dados do Grafana:
    mkdir -p volumes/grafana

##### Criar uma rede do Docker para comunicar com o container:
    docker network create grafana-net

##### Iniciar o Grafana:
    ID=$(id -u)
    docker run -d --user $ID -v /home/vagrant/volumes/grafana/:/var/lib/grafana -p 3000:3000 --name=grafana --network=grafana-net grafana/grafana

##### Acessar o grafana pela rede padr�o que o Vagrant cria:
    http://192.168.33.10:3000
    Usu�rio: admin
    Senha: admin
    Nova senha: mestre

##### Criar o primeiro dashboard:
    Create -> Dashboard -> Add Query
    Visualization -> Mostrar os tipos de visualiza��es mais comuns
    Criar um gr�fico, gauge, bar gauge e table (Nome: Aula1 - Meu primeiro dashboard)


#### Instalando o banco de dados temporal - InfluxDB

##### Criando um container para o InfluxDB
    cd ~
    mkdir /volumes/influxdb
    docker run -d -v /home/vagrant/volumes/influxdb/:/var/lib/influxdb -p 8083:8083 -p 8086:8086 -p 25826:25826/udp --name=influxdb --network=grafana-net influxdb:1.0


##### Coletores de m�tricas - Telegraf

Instala��o do Telegraf conforme documenta��o: [Clique aqui](https://docs.influxdata.com/telegraf/v1.21/introduction/installation/)
Procurar no google por: installing telegraf debian

    wget -qO- https://repos.influxdata.com/influxdb.key | sudo tee /etc/apt/trusted.gpg.d/influxdb.asc >/dev/null
    source /etc/os-release
    echo "deb https://repos.influxdata.com/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
    sudo apt-get update && sudo apt-get install telegraf


##### Verificar se o Telegraf est� iniciado:
    sudo service telegraf status


##### Acessar o container do InfluxDB para verificar se o coletor de m�tricas est� guardando dados nele.
    docker exec -it influxdb bash


##### Conectar no influx e visualizar dados b�sicos do banco:
    # influx
    > show databases
    > use telegraf
    > show measurements


##### Criar novo Datasource no Grafana.
    Criar novo datasource do tipo InfluxDB
    URL do InfluxDB: http://192.168.33.10:8086
    Access: Browser
    Database: telegraf
    Clique em <Save & test>


##### Criar vari�veis no Grafana.
    Ao criar um novo dashboard, � poss�vel criar vari�veis nele.
    Exemplo:
    Name: server
    Label: server
    Type: Query
    Data source: InfluxDB
    Refresh: On Time Range Change
    Query: SHOW TAG VALUES FROM system WITH KEY=host
    No Preview of values j� mostra: ubuntu-bionic

	
##### Instalando a ferramenta para estressar o servidor.

Estressando a CPU do servidor
	$ sudo apt-get update
    $ sudo apt-get install stress-ng
    $ stress-ng -c 0 -l 95
	
Criando um arquivo grande no servidor
    $ dd if=/dev/zero of=arquivo.img bs=1M count=3000
	
Estressando a Mem�ria do servidor
    $ stress-ng --vm-bytes \
    $(awk \
    '/MemAvailable/{printf "%d\n", $2 * 0.9;}' \
    < /proc/meminfo\
    )k \
    --vm-keep -m 1

##### Coletar m�tricas do Docker.

1) Modificar o arquivo /etc/telegraf/telegraf.conf e dar permiss�o para ler o socket do Docker:
    $sudo usermod -aG docker telegraf
	$cat telegraf.conf | grep -A 45 inputs.docker | egrep -v "^#"[[inputs.docker]]

Retirar o coment�rio dos seguintes atributos:
    [[inputs.docker]]
    endpoint = "unix:///var/run/docker.sock"
    container_names = []
    container_name_include = []
    container_name_exclude = []
    timeout = "5s"
    total = false

Reiniciar o telegraf:
	$ sudo service telegraf restart
	$ sudo service telegraf status


##### Coletar dados de um Apache

1) Instalar o Apache:

    $ sudo apt-get install -y apache2

2) Modificar o arquivo /etc/telegraf/telegraf.conf e dar permiss�o para ler os logs do Apache:
    $ sudo usermod -a -G adm telegraf
    $ cat telegraf.conf | grep -A 45 "\[\[inputs.logparser\]\]" | egrep -v "^#"
    [[inputs.logparser]]
    files = ["/var/log/apache2/access.log"]
    from_beginning = true
    [inputs.logparser.grok]
    patterns = ["%{COMBINED_LOG_FORMAT}"]
    measurement = "apache_access_log"
    
	$ sudo service telegraf restart
	$ sudo service telegraf status

3) Criar o dashboard:

    Create > Dashboard > Configura��es
	
4) Criar a vari�vel code para o uso global do dashboard:
    � poss�vel criar vari�veis e referenciar outras vari�veis na query.
    Exemplo:
    Name: code
    Label: code
    Type: Query
    Data source: InfluxDB
    Refresh: On Time Range Change
    Query: SHOW TAG VALUES WITH KEY = "resp_code" where host =~ /^$server$/
	Setar: Multi-Value e Include all options
    No Preview of values j� mostra os valores.

	
5) Criar os pain�is para monitorar o Apache:
    SELECT count("request") FROM "apache_access_log" WHERE "host" =  'ubuntu-bionic' AND "resp_code" = '404' AND $timeFilter  AND "agent" != 'Go-http-client/1.1' AND agent != 'worldping-api'
	SELECT  "request" FROM "apache_access_log" WHERE "host" =~ /^$server$/ AND "resp_code" =~ /^$code$/ AND $timeFilter
    SELECT  "client_ip" FROM "apache_access_log" WHERE "host" =~ /^$server$/  AND "resp_code" =~ /^$code$/ AND $timeFilter
	
	
6) Estressar a aplica��o para visualizar os logs e contagem de erros:
    # Estressando a quantidade de requests
    $ for i in {1..501}; do curl http://localhost/  > /dev/null 2>&1;done

    # Estressando a quantidade de requests invalidos:
    $ for i in {1..501}; do curl http://localhost/alura  > /dev/null 2>&1;done
	

##### Integrando o Slack e Grafana

1) Configurar o bot no canal do Slack
    Adicionar uma APP no canal -> Incoming WebHooks > Add Configuration
    Post to Channel > cursos-alura > Add Incoming WebHooks integration

	
2) Adicionar a configura��o no Grafana
    Alerting > Notification channels > Add channel
    Name: slack
    Type: Slack
    Default
    Url: Slack Webhook URL
    Send Test

	
### Caso a m�quina virtual seja desligada, para ligar novamente:.
    vagrant up
    vagrant ssh
    docker ps -a
    docker start <id_do_container>
