# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  # Boxes to use.
  # Puppet Labs CentOS 6.4 for VirtualBox
  config.vm.provider :virtualbox do |virtualbox, override|
    override.vm.box     = 'centos-71-x64-vbox'
    override.vm.box_url = 'https://github.com/CommanderK5/packer-centos-template/releases/download/0.7.1/vagrant-centos-7.1.box'

    ## memory
    virtualbox.cpus = 1
    virtualbox.memory = 2048
    #virtualbox.customize ["modifyhd", "disk id", "--resize", "size in megabytes"]
  end

  ## build controller node
  config.vm.define "controller", primary: true do |controller|
    ## hostmanager
    controller.vm.hostname = 'controller'
    controller.hostmanager.aliases = %w(controller.local controller)

    # Forward standard ports (local only, does not run under AWS)
    controller.vm.network :forwarded_port, guest: 80,  host: 9080, auto_correct: true
    controller.vm.network :forwarded_port, guest: 443, host: 9443, auto_correct: true

    controller.vm.network :forwarded_port, guest: 5000,  host: 5000, auto_correct: true
    controller.vm.network :forwarded_port, guest: 35357,  host: 35357, auto_correct: true

    controller.vm.network :forwarded_port, guest: 9292,  host: 9292, auto_correct: true

    controller.vm.network :forwarded_port, guest: 8773,  host: 8773, auto_correct: true
    controller.vm.network :forwarded_port, guest: 8774,  host: 8774, auto_correct: true
    controller.vm.network :forwarded_port, guest: 8775,  host: 8775, auto_correct: true
    controller.vm.network :forwarded_port, guest: 8776,  host: 8776, auto_correct: true
    controller.vm.network :forwarded_port, guest: 8777,  host: 8777, auto_correct: true
    controller.vm.network :forwarded_port, guest: 6080,  host: 6080, auto_correct: true

    controller.vm.network :forwarded_port, guest: 9696,  host: 9696, auto_correct: true

    controller.vm.network :forwarded_port, guest: 8000,  host: 8000, auto_correct: true
    controller.vm.network :forwarded_port, guest: 8004,  host: 8004, auto_correct: true

    # provide network interfaces
    controller.vm.network "private_network", ip: "10.10.10.51", virtualbox__intnet: "private"
    controller.vm.network "private_network", ip: "192.168.100.51", virtualbox__intnet: "private"

    controller.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
    end

  end

  ## build network node
  config.vm.define "network_node", primary: true do |network_node|
    ## hostmanager
    network_node.vm.hostname = 'networknode'
    network_node.hostmanager.aliases = %w(networknode.local networknode)
    network_node.vm.network "private_network", ip: "10.10.10.52", virtualbox__intnet: "private"
    network_node.vm.network "private_network", ip: "10.20.20.52", virtualbox__intnet: "private"
    network_node.vm.network "private_network", ip: "192.168.100.52", virtualbox__intnet: "private"
    network_node.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
    end
  end

  (3..3).each do |i|
    config.vm.define "compute-#{i}" do |node|
      ## hostmanager
      node.vm.hostname = "compute-#{i}"
      #node.hostmanager.aliases = %w("compute-#{i}.local" "compute-#{i}")
      node.vm.network "private_network", ip: "10.10.10.5#{i}", virtualbox__intnet: "private"
      node.vm.network "private_network", ip: "10.20.20.5#{i}", virtualbox__intnet: "private"
      node.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/playbook.yml"
      end
    end
  end

end
