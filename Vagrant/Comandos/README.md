# Vagrant - Comandos

### Vagrant
No Vagrant temos:
  - Provider - (Provedores das boxes)
  - Provisioners - (Ferramentas que instalam automaticamente as boxes)

### Boxes dispon�veis:
[https://app.vagrantup.com/boxes/search] [Boxes disponiveis]

### Para subir a VM:
    $ vagrant up

### Para logar na m�quina:
    $ vagrant ssh
    $ vagrant ssh <server_name>

### Para sair da m�quina:
    $ exit
    Atalho: ctrl + d

### Desligar as m�quinas:
    $ vagrant halt

### Destruir seu ambiente:
    $ vagrant destroy
    
    Destr�i sem perguntar:
    $ vagrant destroy -f

### Visualizar as VMs:
    $ vagrant status

### Rodar um comando em um server e depois sair:
    $ vagrant ssh <server_name> -c "cat /etc/*release"

### Fazer o download de um Box espec�fico:
    $ vagrant box add ubuntu/bionic64




   [Boxes disponiveis]: <https://app.vagrantup.com/boxes/search>