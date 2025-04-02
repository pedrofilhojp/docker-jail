# -*- mode: ruby -*-
# vi: set ft=ruby :
# Before execute this Vagrant File, Type the command below in your Linux Terminal
#   export VAGRANT_EXPERIMENTAL="disks"

Vagrant.configure("2") do |config|
    config.vm.box = "roboxes/ubuntu2204"
    config.vbguest.auto_update = false

    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "1024"
    end
  
    # Intasll Docker in the VM
    config.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install apt-transport-https ca-certificates curl software-properties-common -y
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
      sleep 2
      apt-get update
      apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    SHELL

    #
    # Configurations of Student VM
    #
    config.vm.define "Ubuntu22-Virtualizacao" do |aluno|
      aluno.vm.hostname = "Ubuntu22-Virtualizacao"
      aluno.vm.network "private_network", ip: "192.168.57.25"
    #   aluno.vm.network "public_network", bridge: "enp2s0"
  
      aluno.vm.provider "virtualbox" do |vb|
        vb.name = "Ubuntu22-Virtualizacao"
      end
    end # End conf do aluno
end
  