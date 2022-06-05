# Informa��es ref. ao DOCKER

[Clique aqui para baixar o Docker](https://www.docker.com/)

Pr�-Requisitos:
- Arquitetura 64 bits
- Vers�o do Windows Pro, Enterprise ou Education (Se instala��o para Windows).
- Virtualiza��o habilitada

&nbsp;

### Neste passo-a-passo, ser� visto como instalar o Docker no Ubuntu 64 bits. Todos os comandos listados devem ser executados no seu terminal.

&nbsp;

##### Antes de mais nada, remova poss�veis vers�es antigas do Docker:
    sudo apt-get remove docker docker-engine docker.io

##### Depois, atualize o banco de dados de pacotes:
    sudo apt-get update

##### Agora, adicione ao sistema a chave GPG oficial do reposit�rio do Docker:
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

##### Adicione o reposit�rio do Docker as fontes do APT:
    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

##### Atualize o banco de dados de pacotes, para ter acesso aos pacotes do Docker a partir do novo reposit�rio adicionado:
    sudo apt-get update

##### Por fim, instale o pacote docker-ce:
    sudo apt-get install docker-ce

##### Caso voc� queira, voc� pode verificar se o Docker foi instalado corretamente verificando a sua vers�o:
    sudo docker version

##### E para executar o Docker sem precisar de sudo, adicione o seu usu�rio ao grupo docker:
    sudo usermod -aG docker $(whoami)

&nbsp;

## Comandos:

### Verificar a vers�o do docker:
    docker version

### Caso queira executar um container para teste de instala��o:
    docker run hello-world

### Baixa um container, executa e libera o terminal:
    docker run -d dockersamples/static-site

### Baixa um container, executa e libera o terminal e atribui porta local aleat�ria:
    docker run -d -P dockersamples/static-site

### Baixa um container, executa e libera o terminal e atribui porta local espec�fica:
    docker run -d -p 12345:80 dockersamples/static-site

### Baixa um container, executa e libera o terminal e atribui porta local espec�fica e seta vari�vel de ambiente:
    docker run -d -p 12345:80 -e AUTHOR="nome do autor" dockersamples/static-site

### Nomear um container:
    docker run -d -P --name meu-site dockersamples/static-site

### Visualizar as portas utilizadas:
    docker port <id_container>

### Listar containers ativos:
    docker ps

### Listar todos os containers:
    docker ps -a

### Iniciar um container:
    docker start <id_container>
 
### Iniciar e acessar um container linux (por exemplo):
    docker start -a -i <id_container>

### Parar um container:
    docker stop <id_container>

### Remover um container:
    docker rm <id_container>

### Limpar todos os containers inativos:
    docker container prune

### Listar imagens:
    docker images

### Remover imagens:
    docker rmi <nome_imagem> (Exemplo: hello_world>

### Listar todos os ids dos containers:
    docker ps -q

### Parar todos os containers:
    docker stop -t 0 $(docker ps -q)

### Criar volumes:
    docker run -v "/var/www" ubuntu

### Criar volumes especificando o local de armazenamento:
    docker run -v "C:\Volume:/var/www" ubuntu

### Inspecionar o container:
    docker inspect <id_container>

### Executando uma aplica��o node dentro do container:
    docker run -d -p 8080:3000 -v "C:\Volume:/var/www" -w "/var/www" node npm start

### Exemplo de Dockerfile:
    FROM node:latest
    MAINTAINER Douglas Quintanilha
    ENV PORT=3000
    COPY . /var/www
    WORKDIR /var/www
    RUN npm install
    ENTRYPOINT ["npm", "start"]	
    EXPOSE $PORT

### Fazer build de uma imagem:
    docker build -f Dockerfile -t douglasq/node .

### Subindo imagem para o Docker Hub:
    docker login
    <Efetuar o login>
    docker push douglasq/node

### Baixando a imagem do Docker Hub:
    docker pull douglasq/node

### Criando uma network:
    docker network create --driver bridge minha-rede
    docker network ls

### Criando um container com uma rede espec�fica:
    docker run -it --name meu-container-de-ubuntu --network minha-rede ubuntu

### Docker Compose

O Docker Compose n�o � instalado por padr�o no Linux, ent�o voc� deve instal�-lo por fora. Para tal, baixe-o na sua vers�o mais atual, que pode ser visualizada no seu [GitHub](https://github.com/docker/compose/releases), executando o comando abaixo:

    sudo curl -L https://github.com/docker/compose/releases/download/1.15.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

Ap�s isso, d� permiss�o de execu��o para o docker-compose:

    sudo chmod +x /usr/local/bin/docker-compose

Pronto, o Docker Compose j� est� instalado no seu Linux!


### Realizar o build atrav�s do docker compose:
Necess�rio ter o arquivo docker-compose.yml

    docker-compose build

### Executar todo o servi�o do docker compose:
    docker-compose up -d

### Listar todos os servi�os que est�o rodando:
    docker-compose ps

### Parar todos os containers e tamb�m remove os containers:
    docker-compose down

### Reiniciar todos os containers:
    docker-compose restart