# Comandos do GIT

[Clique aqui para baixar o client do GIT](https://git-scm.com)

### Configurações globais:
    git --global user.name "Seu nome completo"
    git --global user.email "seu_email@dominio.com.br"

### Para criar repositório local para depois subir ao repositório remoto:

//Inicia repositório (.git)

    git init


### Para baixar o repositório remoto para a máquina local:
    git clone <URL do repositório>

### Status:
    git status

### Criar uma Branch:
    git checkout -b <nome da branch>

### Para alterar a branch localmente:
    git checkout <nome da branch>

### Adicionar arquivos ao repositório:
    git add <caminho do arquivo>

ou, para adicionar todas as alterações:

    git add .


### Realizar commit das alterações localmente:
    git commit -m "Descrição da alteração"

### Para setar o repositório remoto:
    git -set --upstream https://github.com/sua_conta/repositorio.git

### Para subir as alterações para o repositório remoto:
    git push origin <nome da branch>

Exemplo:

    git push origin master


### Para baixar os arquivos do repositório remoto para o repositório local:
    git pull

### Mostrar histórico:
    git log

Mostrar o histórico em uma linha:

    git log --oneline


### Configurando o proxy no GIT:

Para habilitar:

    git config --global http.proxy http://domain\user:password@proxy.com.br:8080

Para desabilitar:

    git config --global --unset http.proxy