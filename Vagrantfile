# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.provision "shell", inline: <<-SHELL
     sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine
     sudo yum install -y yum-utils device-mapper-persistent-data lvm2
     sudo yum-config-manager -y --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     sudo yum install -y docker-ce
     sudo gpasswd -a vagrant docker
     sudo systemctl start docker
  SHELL
end
