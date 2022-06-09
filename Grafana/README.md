# Informações ref. ao GRAFANA

Preparação do ambiente:
- VirtualBox - Versão: 6.1.34
- Vagrant [Clique aqui para baixar o Vagrant](https://www.vagrantup.com/)
- Cmder [Clique aqui para baixar o Cmder](https://cmder.net/)

&nbsp;

### Neste passo-a-passo, será visto como instalar o Grafana utilizando uma máquina virtual através do Vagrant.

&nbsp;

##### Subindo a máquina virtual:

Colocar o arquivo Vagrantfile (que se encontra neste projeto dentro da pasta Arquivos) dentro de uma pasta e dentro da pasta executar:
    
    vagrant up

##### Acessar a máquina virtual:
    vagrant ssh

##### Atualizar os pacotes do Linux.
    sudo apt-get update && sudo apt-get upgrade -y

##### Baixar o script oficial do Docker para instalar um container do Grafana.
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh

##### Adicionar o usuário vagrant no grupo do docker:
    sudo usermod -aG docker vagrant

##### Após este comando, sair da máquina e logar novamente:
    exit
    vagrant ssh

##### Testando a instalação:
    docker ps

##### Criar pasta para o volume para persistir os dados do Grafana:
    mkdir -p volumes/grafana

##### Criar uma rede do Docker para comunicar com o container:
    docker network create grafana-net

##### Iniciar o Grafana:
    ID=$(id -u)
    docker run -d --user $ID -v /home/vagrant/volumes/grafana/:/var/lib/grafana -p 3000:3000 --name=grafana --network=grafana-net grafana/grafana

##### Acessar o grafana pela rede padrão que o Vagrant cria:
    http://192.168.33.10:3000
    Usuário: admin
    Senha: admin
    Nova senha: mestre

##### Criar o primeiro dashboard:
    Create -> Dashboard -> Add Query
    Visualization -> Mostrar os tipos de visualizações mais comuns
    Criar um gráfico, gauge, bar gauge e table (Nome: Aula1 - Meu primeiro dashboard)

