# Informa��es ref. ao MONGODB


&nbsp;

### Ferramenta visual para MongoDB (Robomongo):
[Clique aqui para baixar o Robomongo](https://robomongo.org/download)

&nbsp;

### Neste passo-a-passo, ser� visto como instalar o MongoDB utilizando uma m�quina virtual atrav�s do Vagrant.

&nbsp;

##### Subindo a m�quina virtual:

Colocar o arquivo Vagrantfile (que se encontra neste projeto dentro da pasta Arquivos) dentro de uma pasta e dentro da pasta executar:
    
    vagrant up

##### Acessar a m�quina virtual:
    vagrant ssh

##### Atualizar os pacotes do Linux.
    sudo apt-get update && sudo apt-get upgrade -y

##### Baixar o script oficial do Docker para instalar um container do MongoDB.
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh

##### Adicionar o usu�rio vagrant no grupo do docker:
    sudo usermod -aG docker vagrant

##### Ap�s este comando, sair da m�quina e logar novamente:
    exit
    vagrant ssh

##### Testando a instala��o:
    docker ps
    
##### Baixar a imagem do mongo para o Docker.
    $ sudo docker pull mongo

##### Certificar que a imagem est� instalada.
    sudo docker images
    
##### Iniciar o container do MongoDB.
    sudo docker run -p 27017:27017 --name mongo_server -d mongo
    
Verificar logs:

    sudo docker logs <ID_CONTAINER>
    
Em caso de erro:
WARNING: MongoDB 5.0+ requires a CPU with AVX support, and your current system does not appear to have that!
Utilizar vers�o mais antiga do MongoDB.
    
    sudo docker run -p 27017:27017 --name mongo_server -d mongo:4.4.6

##### Verificar se container est� iniciado.
    sudo docker ps
    
##### Conectar no MongoDB pela rede padr�o que o Vagrant cria:
    192.168.33.10:27017