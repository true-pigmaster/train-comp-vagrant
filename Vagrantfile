# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  # config.vm.box = "hashicorp/bionic64"
  #config.vm.box_version = "v1.0.282"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  # config.vm.box_url = "hashicorp/bionic64"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  #  config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "hyperv"
  # config.vm.provider "hyperv" do |h|
    #  h.enable_virtualization_extensions = true
    # h.linked_clone = true
    # h.cpus = "1"
    # memory = "1024"
  # end
  # web1.vm.provider:hyperv do |h1|
    # h1.enable_virtualization_extensions = true
    # h1.linked_clone = true
    # h1.cpus = "1"
    # h1.memory = "1024"
    # h1.vmname = "web1"
    # h1.enable_checkpoints = false
  # end
    # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
   #config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y ansible
  #  SHELL
  # config.vm.provider "virtualbox" do |vb|
    #  vb.gui = true     # Display the VirtualBox GUI when booting the machine     
    #  vb.memory = "1024" #Customize the amount of memory on the VM
  # end
  config.vm.define "web1" do |web1|
    web1.vm.box = "hashicorp/bionic64"
    web1.vm.synced_folder ".", "/vagrant", disabled: true
    web1.vm.hostname = "web1"
    web1.vm.provision "shell", inline: <<-SHELL
    SHELL
  end
  config.vm.define "web2" do |web2|
    web2.vm.box = "hashicorp/bionic64"
    web2.vm.synced_folder ".", "/vagrant", disabled: true
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "10.0.2.10", virtualbox__intnet: true
    config.vm.provider "virtualbox" do |wvb2|
      wvb2.gui = true     
      wvb2.memory = "1024" 
      wvb2.name = "web2"
      wvb2.linked_clone = true
      wvb2.cpus = 2
   end
    web2.vm.provision "shell", inline: <<-SHELL
    SHELL
  end
  config.vm.define "control" do |control|
    control.vm.box = "centos/7"
    control.vm.synced_folder ".", "/vagrant", disabled: true
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: "10.0.2.5", virtualbox__intnet: true
    config.vm.provider "virtualbox" do |cvb|
      cvb.gui = true     
      cvb.memory = "1024" 
      cvb.name = "control"
      cvb.linked_clone = true
      cvb.cpus = 2
   end
    control.vm.provision "shell", inline: <<-SHELL
     yum install ansible -y
    SHELL
  end
end

# ToDo: Install virtualbox guest additions
# yum install wget -y
# yum install kernel-devel build-essential dkms
# wget http://download.virtualbox.org/virtualbox/6.1.2/VBoxGuestAdditions_6.1.2.iso
# sudo mkdir /media/VBoxGuestAdditions
# sudo mount -o loop,ro VBoxGuestAdditions_6.1.2.iso /media/VBoxGuestAdditions
# sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
# rm VBoxGuestAdditions_6.1.2.iso
# sudo umount /media/VBoxGuestAdditions
# sudo rmdir /media/VBoxGuestAdditions