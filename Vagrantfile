# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile for ansible-common-role

Vagrant.configure("2") do |config|
  # config.vm.network 'forwarded_port', guest: 80, host: 8080
  config.vm.synced_folder '.', '/vagrant', disabled: false
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
    apt-get update -y
  SHELLALL

  # Note: this doesn't work on Debian 8/Jessie
  config.vm.define "pxe9" do |pxe9|
    pxe9.vm.box = "debian/stretch64" #"geerlingguy/debian9"
    pxe9.ssh.insert_key = false
    pxe9.vm.network 'private_network', ip: '192.168.10.9'
    pxe9.vm.hostname = 'pxe9'
    
    pxe9.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python
    SHELL
    pxe9.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
      # ansible.verbose = "v"
      # ansible.raw_arguments = [""]
    end
  end

# ubuntu/xenial64     (virtualbox, 20190530.3.0)
# ubuntu/bionic64     (virtualbox, 20190531.0.0)

  config.vm.define "pxe16" do |pxe16|
    pxe16.vm.box = "ubuntu/xenial64"
    pxe16.ssh.insert_key = false
    pxe16.vm.network 'private_network', ip: '192.168.10.16'
    pxe16.vm.hostname = 'pxe16'

    pxe16.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python
    SHELL
    pxe16.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
    end
  end

  config.vm.define "pxe18" do |pxe18|
    pxe18.vm.box = "ubuntu/bionic64"
    pxe18.ssh.insert_key = false
    pxe18.vm.network 'private_network', ip: '192.168.10.18'
    pxe18.vm.hostname = 'pxe18'

    pxe18.vm.provision "shell", inline: <<-SHELL
      apt-get install -y python
      apt -y autoremove
    SHELL
    pxe18.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory"
    end
  end

end
