# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_docker_script = <<SCRIPT
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker vagrant
SCRIPT

Vagrant.configure('2') do |config|
vm_box = 'ubuntu/xenial64'
config.vm.define :manager, primary: true  do |manager|
    manager.vm.box = vm_box
    manager.vm.box_check_update = true
    manager.vm.network :private_network, ip: "10.100.199.200"
    manager.vm.network :forwarded_port, guest: 8080, host: 8080
    manager.vm.network :forwarded_port, guest: 5000, host: 5000
    manager.vm.hostname = "manager"
    manager.vm.synced_folder ".", "/vagrant"
    manager.vm.provision "shell", inline: $install_docker_script, privileged: true
    manager.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.name = "manager"
      vb.memory = "1024"
    end
  end
(1..3).each do |i|
    config.vm.define "worker0#{i}" do |worker|
      worker.vm.box = vm_box
      worker.vm.box_check_update = true
      worker.vm.network :private_network, ip: "10.100.199.20#{i}"
      worker.vm.hostname = "worker0#{i}"
      worker.vm.synced_folder ".", "/vagrant"
      worker.vm.provision "shell", inline: $install_docker_script, privileged: true
      worker.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.name = "worker0#{i}"
        vb.memory = "1024"
      end
    end
  end
end
