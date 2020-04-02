# -*- mode: ruby -*-
# vi: set ft=ruby :
CONTROL_IP = "10.10.0.1"
WEB1_IP = "10.10.0.11"
WEB2_IP = "10.10.0.12"
DB1_IP = "10.10.0.21"
DB2_IP = "10.10.0.22"

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.box_version = "1.0.282"
  config.ssh.insert_key = "false"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
    vb.linked_clone = true
    vb.cpus = 1
  end
  config.vm.define "web1" do |web1|
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: WEB1_IP, virtualbox__intnet: true
    web1.vm.network "forwarded_port", guest: 8080, host: 8081, host_ip: "127.0.0.1"
    web1.vm.provider "virtualbox" do |wvb1|
      wvb1.name = "web1"
   end
  end
  config.vm.define "web2" do |web2|
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: WEB2_IP, virtualbox__intnet: true
    web2.vm.network "forwarded_port", guest: 8080, host: 8082, host_ip: "127.0.0.1"
    web2.vm.provider "virtualbox" do |wvb2|
      wvb2.name = "web2"
   end
  end
  config.vm.define "db1" do |web1|
    web1.vm.hostname = "db1"
    web1.vm.network "private_network", ip: DB1_IP, virtualbox__intnet: true
    web1.vm.provider "virtualbox" do |dvb1|
      dvb1.name = "db1"
   end
  end
  config.vm.define "control" do |control|
    control.vm.box = "centos/7"
    control.vm.box_version = "1905.1"
    control.vm.hostname = "control"
    control.vm.network "private_network", ip: CONTROL_IP, virtualbox__intnet: true
    control.vm.provision "file", source: "/Projects/Vagrant/keys/ansible_key", destination: "/home/vagrant/.ssh/ansible_key"
    control.vm.provider "virtualbox" do |cvb|
      cvb.name = "control"
      cvb.cpus = 2
    end
    control.vm.provision "shell", inline: <<-SHELL
     yum install ansible -y
     yum install nano -y 
     chmod 600 /home/vagrant/.ssh/ansible_key
     cat /vagrant/ansible/hosts > /etc/ansible/hosts
    SHELL
  end
  config.vm.provision "shell", args: [WEB1_IP, WEB2_IP, DB1_IP, DB2_IP, CONTROL_IP], inline: <<-SHELL
    echo "$1  web1\n$2  web2\n$3  db1\n$4 db2\n$5 control" >> /etc/hosts
    cat /vagrant/keys/ansible_key.pub >> /home/vagrant/.ssh/authorized_keys
    chmod 700 /home/vagrant/.ssh
    chmod 640 /home/vagrant/.ssh/authorized_keys
  SHELL
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