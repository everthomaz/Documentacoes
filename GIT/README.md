# Comandos do GIT

[Clique aqui para baixar o client do GIT](https://git-scm.com)

### Para instalar o git no linux:
    sudo yum install git -y

### Configura��es globais:
    git config --global user.name "Seu nome completo"
    git config --global user.email "seu_email@dominio.com.br"
    git config --global core.editor vi

### Usar o comando abaixo para configurar o seu gitBash a aceitar longos caminhos de diret�rios:
    git config --system core.longpaths true

### Configurando o proxy no GIT:

Para habilitar:

    git config --global http.proxy http://domain\user:password@proxy.com.br:8080

Para desabilitar:

    git config --global --unset http.proxy

### Para listar as configura��es:
    git config --list

### Para criar reposit�rio local para depois subir ao reposit�rio remoto:

//Inicia reposit�rio (.git)

    git init

### Para baixar o reposit�rio remoto para a m�quina local:
    git clone <URL do reposit�rio>
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
Obs.: Caso tenha ocorrido mudan�as na branch, ir� solicitar uma confirma��o alterando o par�metro para -D "mai�sculo".

### Adicionar arquivos ao reposit�rio:
    git add <caminho do arquivo>

ou, para adicionar todas as altera��es:

    git add .


### Realizar commit das altera��es localmente:
    git commit -m "Descri��o da altera��o"

### Para incluir um novo arquivo no �ltimo commit:
    git commit --amend
Obs.: Vai abrir o editor, pode salvar e fechar.

### Para setar o reposit�rio remoto:
    git -set --upstream https://github.com/sua_conta/repositorio.git

### Para subir as altera��es para o reposit�rio remoto:
    git push origin <nome da branch>

Exemplo:

    git push origin master
    git push --set-upstream origin <nome da branch>


### Realizar o reset do reposit�rio local:
    git reset --hard origin/20200517-E1

### Para baixar os arquivos do reposit�rio remoto para o reposit�rio local:
    git pull

### Mostrar hist�rico:
    git log

Mostrar o hist�rico em uma linha:

    git log --oneline
