# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "chef/centos-6.5"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    sudo yum update -y
    sudo yum install -y rpm-build redhat-rpm-config spectool gcc-c++
    mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
    echo '%_topdir %(echo $HOME)/rpmbuild' > ~/.rpmmacros
    cp /vagrant/{*.patch,*.spec,LICENSE} ~/rpmbuild/SOURCES
    sudo yum install -y yum-utils --enablerepo=extras
    spectool -g -R SOURCES/js.spec
    rpmbuild -bs SOURCES/js.spec
    cd ~/rpmbuild && sudo yum-builddep -y SRPMS/js-1.8.5-15.el6.src.rpm
    rpmbuild --rebuild SRPMS/js-1.8.5-15.el6.src.rpm
    cp RPMS/x86_64/js-* /vagrant/
  SHELL
end
