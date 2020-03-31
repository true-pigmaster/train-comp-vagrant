# -*- mode: ruby -*-
# vi: set ft=ruby :
CONTROL_IP = "10.10.0.1"
WEB1_IP    = "10.10.0.2"
WEB2_IP    = "10.10.0.3"


Vagrant.configure("2") do |config|
  
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.define "web1" do |web1|
    web1.vm.box = "hashicorp/bionic64"
    # web1.vm.synced_folder ".", "/vagrant", disabled: true
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: WEB1_IP, virtualbox__intnet: true
    web1.ssh.insert_key = "false"
    web1.vm.provider "virtualbox" do |wvb1|
      wvb1.gui = true     
      wvb1.memory = "1024" 
      wvb1.name = "web1"
      wvb1.linked_clone = true
      wvb1.cpus = 1
   end
    web1.vm.provision "shell", inline: <<-SHELL
      echo $CONTROL_IP "control" >> "/etc/hosts"
      cat /vagrant/keys/ansible_key.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end
  config.vm.define "web2" do |web2|
    web2.vm.box = "hashicorp/bionic64"
    # web2.vm.synced_folder ".", "/vagrant", disabled: true
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: WEB2_IP, virtualbox__intnet: true
    web2.ssh.insert_key = "false"
    # web2.vm.provision "file", source: "keys/ansible_key.pub", destination: "/home/vagrant/.ssh/authorized_keys"
    web2.vm.provider "virtualbox" do |wvb2|
      wvb2.gui = true     
      wvb2.memory = "1024" 
      wvb2.name = "web2"
      wvb2.linked_clone = true
      wvb2.cpus = 1
   end
    web2.vm.provision "shell", inline: <<-SHELL
      cat /vagrant/keys/ansible_key.pub >> /home/vagrant/.ssh/authorized_keys
      chmod 700 /home/vagrant/.ssh
      chmod 640 /home/vagrant/.ssh/authorized_keys
      echo "$CONTROL_IP control" >> /etc/hosts
    SHELL
  end

  config.vm.define "control" do |control|
    control.vm.box = "centos/7"
    # control.vm.synced_folder ".", "/vagrant", disabled: true
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: CONTROL_IP, virtualbox__intnet: true
    control.ssh.insert_key = "false"
    control.vm.provision "file", source: "/Projects/Vagrant/keys/ansible_key", destination: "/home/vagrant/.ssh/ansible_key"
    control.vm.provider "virtualbox" do |cvb|
      cvb.gui = true     
      cvb.memory = "1024" 
      cvb.name = "control"
      # cvb.customize ["modifyvm", :id,"--name", "control"]
      cvb.linked_clone = true
      cvb.cpus = 2
    end
    control.vm.provision "shell", inline: <<-SHELL
     yum install ansible -y
     yum install nano -y 
     echo "$WEB2_IP web2" >> /etc/hosts
     echo "$WEB1_IP web1" >> /etc/hosts
     chmod 640 /home/vagrant/.ssh/ansible_key
     echo "[webservers]\nweb1\nweb2\n\n[webservers:vars]\nansible_connection=ssh\nansible_port=22\nansible_ssh_private_key_file=/home/vagrant/.ssh/ansible_key\nansible_user=vagrant" >> /etc/ansible/hosts
    #  cat keys/ansible_key >> /home/vagrant/.ssh/ansible_key
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