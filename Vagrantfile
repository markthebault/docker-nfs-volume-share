# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

BOXE_IMG="ubuntu/trusty64"
IP_SERVER="192.168.100.100"
IP_CLIENT="192.168.100.200"

Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

  config.vm.define "nfs-server" do |nfsserver|
    nfsserver.vm.box = BOXE_IMG
    nfsserver.vm.hostname = "nfs-server"
    nfsserver.vm.network "private_network", ip: IP_SERVER

    nfsserver.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      #Install NFS Server
      sudo apt-get install -y nfs-kernel-server
      sudo mkdir /var/nfs/general -p
      sudo chown nobody:nogroup /var/nfs/general
      sudo echo "/var/nfs/general    *(rw,sync,no_subtree_check)" >/etc/exports
      sudo echo "/home       *(rw,sync,no_root_squash,no_subtree_check)" >>/etc/exports
      sudo service nfs-kernel-server restart
    SHELL
  end



  config.vm.define "nfs-client" do |nfsclient|
    nfsclient.vm.box = BOXE_IMG
    nfsclient.vm.hostname = "nfs-client"
    nfsclient.vm.network "private_network", ip: IP_CLIENT

    nfsclient.vm.provision "shell", inline: <<-SHELL
      #Install docker
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
      sudo apt-get update
      sudo apt-get install -y docker-ce
      sudo gpasswd -a vagrant docker
      sudo service docker restart


      #Install NFS docker plugin
      wget https://github.com/ContainX/docker-volume-netshare/releases/download/v0.20/docker-volume-netshare_0.20_amd64.deb
      sudo dpkg -i docker-volume-netshare_0.20_amd64.deb

      #Install NFS Server
      sudo apt-get install -y nfs-common

      #Mount NFS volumes
      sudo mkdir -p /nfs/general
      sudo mkdir -p /nfs/home
      sudo mount "#{IP_SERVER}":/var/nfs/general /nfs/general
      sudo mount "#{IP_SERVER}":/home /nfs/home
    SHELL
  end


end
