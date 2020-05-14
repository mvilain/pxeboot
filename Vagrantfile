# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile for ansible-common-role

Vagrant.configure("2") do |config|
  # config.vm.network 'forwarded_port', guest: 80, host: 8080
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.provider :virtualbox do |vb|
    #vb.gui = true
    vb.memory = '1024'
  end
  #
  # provision on all machines to allow ssh w/o checking
  #
  config.vm.provision "shell", inline: <<-SHELLALL
    echo "...disabling CheckHostIP..."
    sed -i.orig -e "s/#   CheckHostIP yes/CheckHostIP no/" /etc/ssh/ssh_config
    for i in /etc/sysconfig/network-scripts/ifcfg-eth1 /etc/sysconfig/network-scripts/ifcfg-enp0s8; do
      if [ -e ${i} ]; then echo "...displaying ${i}..."; cat ${i}; fi
    done
#     apt-get update -y
  SHELLALL

  # Note: this doesn't work on Debian 8/Jessie
  config.vm.define "d9" do |d9|
    d9.vm.box = "debian/stretch64"
    d9.ssh.insert_key = false
    d9.vm.network 'private_network', ip: '192.168.10.9'
    d9.vm.hostname = 'd9'
    
    d9.vm.provision "shell", inline: <<-SHELL
#       apt-get install -y python
    SHELL
    d9.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
      # ansible.verbose = "v"
      # ansible.raw_arguments = [""]
    end
  end

  config.vm.define "d10" do |d10|
    d10.vm.box = "debian/buster64"
    d10.ssh.insert_key = false
    d10.vm.network 'private_network', ip: '192.168.10.10'
    d10.vm.hostname = 'd10'
    
    d10.vm.provision "shell", inline: <<-SHELL
#       apt-get install -y python
    SHELL
    d10.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
    end
  end

  config.vm.define "u16" do |u16|
    u16.vm.box = "ubuntu/xenial64"
    u16.ssh.insert_key = false
    u16.vm.network 'private_network', ip: '192.168.10.16'
    u16.vm.hostname = 'u16'

    u16.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python
    SHELL
    u16.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
    end
  end

  config.vm.define "u18" do |u18|
    u18.vm.box = "ubuntu/bionic64"
    u18.ssh.insert_key = false
    u18.vm.network 'private_network', ip: '192.168.10.18'
    u18.vm.hostname = 'u18'

    u18.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python
      apt -y autoremove
    SHELL
    u18.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
    end
  end

end
