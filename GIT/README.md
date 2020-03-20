# Comandos do GIT

[Clique aqui para baixar o client do GIT](https://git-scm.com)

### Para instalar o git no linux:
    sudo yum install git -y

### Configurações globais:
    git config --global user.name "Seu nome completo"
    git config --global user.email "seu_email@dominio.com.br"
    git config --global core.editor vi

### Usar o comando abaixo para configurar o seu gitBash a aceitar longos caminhos de diretórios:
    git config --system core.longpaths true

### Configurando o proxy no GIT:

Para habilitar:

    git config --global http.proxy http://domain\user:password@proxy.com.br:8080

Para desabilitar:

    git config --global --unset http.proxy

### Para listar as configurações:
    git config --list

### Para criar repositório local para depois subir ao repositório remoto:

//Inicia repositório (.git)

    git init

### Para baixar o repositório remoto para a máquina local:
    git clone <URL do repositório>
Exemplo: git clone https://github.com/leachim6/hello-world.git

### Status:
    git status

### Criar uma Branch:
    git checkout -b <nome da branch>
ou

    git branch <nome da branch>

### Para alterar a branch localmente:
    git checkout <nome da branch>

### Listar as branchs:
    git branch

### Para remover uma branch:
    git branch -d <nome da branch>
Obs.: Caso tenha ocorrido mudanças na branch, irá solicitar uma confirmação alterando o parâmetro para -D "maiúsculo".

### Adicionar arquivos ao repositório:
    git add <caminho do arquivo>

ou, para adicionar todas as alterações:

    git add .


### Realizar commit das alterações localmente:
    git commit -m "Descrição da alteração"

### Para incluir um novo arquivo no último commit:
    git commit --amend
Obs.: Vai abrir o editor, pode salvar e fechar.

### Para setar o repositório remoto:
    git -set --upstream https://github.com/sua_conta/repositorio.git

### Para subir as alterações para o repositório remoto:
    git push origin <nome da branch>

Exemplo:

    git push origin master
    git push --set-upstream origin <nome da branch>


### Realizar o reset do repositório local:
    git reset --hard origin/20200517-E1

### Para baixar os arquivos do repositório remoto para o repositório local:
    git pull

### Mostrar histórico:
    git log

Mostrar o histórico em uma linha:

    git log --oneline
