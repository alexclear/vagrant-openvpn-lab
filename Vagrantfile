# -*- mode: ruby -*-
# vi: set ft=ruby :

$OVPN1_IP = "172.16.137.92"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/ovpn1/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "ovpn1" do |ovpn1|
    ovpn1.vm.box = "ubuntu/xenial64"
    ovpn1.vm.hostname = "ovpn1"
    ovpn1.vm.network "private_network", ip: $OVPN1_IP

    ovpn1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    ovpn1.vm.provision "shell", inline: "apt-get install -y python"
    ovpn1.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
