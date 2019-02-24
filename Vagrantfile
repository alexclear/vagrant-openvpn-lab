# -*- mode: ruby -*-
# vi: set ft=ruby :

$OVPN1_IP = "172.16.137.92"

ANSIBLE_RAW_SSH_ARGS = []

Vagrant.configure("2") do |config|
  config.vm.define "ovpn1" do |ovpn1|
    ovpn1.vm.box = "generic/ubuntu1604"
    ovpn1.vm.hostname = "ovpn1"
    ovpn1.vm.network "private_network", ip: $OVPN1_IP
    ovpn1.vm.network "forwarded_port", guest: 1195, host: 1195

    ovpn1.vm.provider :virtualbox do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/ovpn1/virtualbox/private_key "
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    ovpn1.vm.provider :libvirt do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/ovpn1/libvirt/private_key "
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.cpus = 2
      v.memory = 4096
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
