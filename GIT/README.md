# Comandos do GIT

[Clique aqui para baixar o client do GIT](https://git-scm.com)

### Configura��es globais:
    git --global user.name "Seu nome completo"
    git --global user.email "seu_email@dominio.com.br"

### Para criar reposit�rio local para depois subir ao reposit�rio remoto:

//Inicia reposit�rio (.git)

    git init


### Para baixar o reposit�rio remoto para a m�quina local:
    git clone <URL do reposit�rio>

### Status:
    git status

### Criar uma Branch:
    git checkout -b <nome da branch>

### Para alterar a branch localmente:
    git checkout <nome da branch>

### Adicionar arquivos ao reposit�rio:
    git add <caminho do arquivo>

ou, para adicionar todas as altera��es:

    git add .


### Realizar commit das altera��es localmente:
    git commit -m "Descri��o da altera��o"

### Para setar o reposit�rio remoto:
    git -set --upstream https://github.com/sua_conta/repositorio.git

### Para subir as altera��es para o reposit�rio remoto:
    git push origin <nome da branch>

Exemplo:

    git push origin master


### Para baixar os arquivos do reposit�rio remoto para o reposit�rio local:
    git pull

### Mostrar hist�rico:
    git log

Mostrar o hist�rico em uma linha:

    git log --oneline


### Configurando o proxy no GIT:

Para habilitar:

    git config --global http.proxy http://domain\user:password@proxy.com.br:8080

Para desabilitar:

    git config --global --unset http.proxy