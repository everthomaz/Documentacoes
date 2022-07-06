# Informações ref. ao MONGODB


&nbsp;

### Ferramenta visual para MongoDB (Robomongo):
[Clique aqui para baixar o Robomongo](https://robomongo.org/download)

&nbsp;

### Neste passo-a-passo, será visto como instalar o MongoDB utilizando uma máquina virtual através do Vagrant.

&nbsp;

##### Subindo a máquina virtual:

Colocar o arquivo Vagrantfile (que se encontra neste projeto dentro da pasta Arquivos) dentro de uma pasta e dentro da pasta executar:
    
    vagrant up

##### Acessar a máquina virtual:
    vagrant ssh

##### Atualizar os pacotes do Linux.
    sudo apt-get update && sudo apt-get upgrade -y

##### Baixar o script oficial do Docker para instalar um container do MongoDB.
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh

##### Adicionar o usuário vagrant no grupo do docker:
    sudo usermod -aG docker vagrant

##### Após este comando, sair da máquina e logar novamente:
    exit
    vagrant ssh

##### Testando a instalação:
    docker ps
    
##### Baixar a imagem do mongo para o Docker.
    $ sudo docker pull mongo

##### Certificar que a imagem está instalada.
    sudo docker images
    
##### Iniciar o container do MongoDB.
    sudo docker run -p 27017:27017 --name mongo_server -d mongo
    
Verificar logs:

    sudo docker logs <ID_CONTAINER>
    
Em caso de erro:
WARNING: MongoDB 5.0+ requires a CPU with AVX support, and your current system does not appear to have that!
Utilizar versão mais antiga do MongoDB.
    
    sudo docker run -p 27017:27017 --name mongo_server -d mongo:4.4.6

##### Verificar se container está iniciado.
    sudo docker ps
    
##### Conectar no MongoDB pela rede padrão que o Vagrant cria:
    192.168.33.10:27017