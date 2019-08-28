# Maquina 1
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provision "shell", inline: <<-SHELL
      curl -sSL https://get.docker.com/ | sh
      usermod -aG docker vagrant
      systemctl enable docker
      init 6
    SHELL

# Configurando Maquinas Virtuais
	config.vm.define "docker10" do |docker10|
		docker10.vm.network "private_network", ip: "192.168.1.10", virtualbox__intnet: true
		docker10.vm.hostname = "docker10"
    docker10.vm.provider "virtualbox" do |v|
     v.gui = false #ativa o modo "grafico"
     v.name = "docker10"
     v.memory = 1024
     v.cpus = 1
   end
  end

	config.vm.define "docker11" do |docker11|
		docker11.vm.network "private_network", ip: "192.168.1.11", virtualbox__intnet: true
		docker11.vm.hostname = "docker11"
    docker11.vm.provider "virtualbox" do |v|
     v.gui = false
     v.name = "docker11"
     v.memory = 1024
     v.cpus = 1
   end
  end

  config.vm.define "docker12" do |docker12|
		docker12.vm.network "private_network", ip: "192.168.1.12", virtualbox__intnet: true
		docker12.vm.hostname = "docker12"
    docker12.vm.provider "virtualbox" do |v|
     v.gui = false
     v.name = "docker12"
     v.memory = 1024
     v.cpus = 1
   end
  end
end
