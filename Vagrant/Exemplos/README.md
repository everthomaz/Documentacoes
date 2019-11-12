# Vagrant - Exemplos

### Criar o Vagrantfile:
Criar o arquivo Vagrantfile onde ficarão as configurações:

    $ vi Vagrantfile

### Para setar a syntax ruby no VI:
    :set syntax=ruby

### Primeiro servidor:
    Vagrant.configure("2") do |config|
      config.vm.box = "centos/7"
      config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = "2"
      end
    end

### Configuração simples para subir 2 servidores:
    Vagrant.configure("2") do |config|
      config.vm.define "server1" do |server1|
        server1.vm.box = "centos/7"
      end
      config.vm.define "server2" do |server2|
        server2.vm.box = "debian/buster64"
      end
    end



### Setando o IP:
    Vagrant.configure("2") do |config|
      config.vm.define "server1" do |server1|
        server1.vm.box = "centos/7"
        server1.vm.network "private_network", ip: "10.2.25.10"
      end
      config.vm.define "server2" do |server2|
        server2.vm.box = "debian/buster64"
        server2.vm.network "private_network", ip: "10.2.25.20"
      end
    end

### Para visualizar o IP no servidor:
    $ ip -c a show eth1
ou

    $ ifconfig -a

### Utilizando variáveis:
    NETWORK = "10.5.25."
    BOX_SRV1 = "centos/7"
    BOX_SRV2 = "ubuntu/bionic64"
    Vagrant.configure("2") do |config|
      config.vm.define "server1" do |server1|
        server1.vm.box = BOX_SRV1
        server1.vm.network "private_network", ip: NETWORK+"10"
      end
      config.vm.define "server2" do |server2|
        server2.vm.box = BOX_SRV2
        server2.vm.network "private_network", ip: NETWORK+"20"
      end
    end


### Exemplo 1 - Subindo 5 instâncias utilizando lista:
    machines = {
        "scm"               => {"image" => "debian/buster64"},
        "automation"        => {"image" => "centos/7"},
        "compliance"        => {"image" => "ubuntu/bionic64"},
        "log"               => {"image" => "ubuntu/bionic64"},
        "container"         => {"image" => "centos/7"}
    }
    
    Vagrant.configure("2") do |config|
      machines.each do |name,conf|
        config.vm.define "#{name}" do |machine|
          machine.vm.box = "#{conf["image"]}"
          machine.vm.hostname = "#{name}.4labs.example"
        end
      end
    end


### Exemplo 2 - Subindo 5 instâncias utilizando lista com opções customizadas do virtualbox:
    machines = {
      "scm"         => {"memory" => "256", "cpu" => "1", "ip" => "40", "image" => "debian/buster64"},
      "automation"  => {"memory" => "3072", "cpu" => "2", "ip" => "10", "image" => "centos/7"},
      "compliance"  => {"memory" => "2048", "cpu" => "2", "ip" => "20", "image" => "ubuntu/bionic64"},
      "log"         => {"memory" => "1024", "cpu" => "1", "ip" => "50", "image" => "ubuntu/bionic64"},
      "container"   => {"memory" => "2048", "cpu" => "1", "ip" => "30", "image" => "centos/7"}
    }
    
    Vagrant.configure("2") do |config|
      config.vm.box_check_update = false
      machines.each do |name,conf|
        config.vm.define "#{name}" do |machine|
          machine.vm.box = "#{conf["image"]}"
          machine.vm.hostname = "#{name}.4labs.example"
          machine.vm.network "private_network", ip: "10.5.25.#{conf["ip"]}"
          machine.vm.provider "virtualbox" do |vb|
            vb.name = "#{name}"
            vb.memory = conf["memory"]
            vb.cpus = conf["cpu"]
            vb.customize ["modifyvm", :id, "--groups", "/525-InfraAgil"]
            if name == "automation" and not File.file?('iscsi525.vdi')
              vb.customize ['createhd', '--filename', 'iscsi525.vdi', '--size', 10240]
              vb.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata']
              vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', 'iscsi525.vdi']
            end
          end
        end
      end
    end