# Vagrant - Comandos

### Vagrant
No Vagrant temos:
  - Provider - (Provedores das boxes)
  - Provisioners - (Ferramentas que instalam automaticamente as boxes)

### Boxes disponíveis:
[https://app.vagrantup.com/boxes/search] [Boxes disponiveis]

### Para subir a VM:
    $ vagrant up

### Para logar na máquina:
    $ vagrant ssh
    $ vagrant ssh <server_name>

### Para sair da máquina:
    $ exit
    Atalho: ctrl + d

### Desligar as máquinas:
    $ vagrant halt

### Destruir seu ambiente:
    $ vagrant destroy
    
    Destrói sem perguntar:
    $ vagrant destroy -f

### Visualizar as VMs:
    $ vagrant status

### Rodar um comando em um server e depois sair:
    $ vagrant ssh <server_name> -c "cat /etc/*release"

### Fazer o download de um Box específico:
    $ vagrant box add ubuntu/bionic64




   [Boxes disponiveis]: <https://app.vagrantup.com/boxes/search>