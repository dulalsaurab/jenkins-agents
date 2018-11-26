# -*- mode: ruby -*-
# vi: set ft=ruby :

$setup_ubuntu = <<EOF
sudo apt-get update
sudo apt-get install -y git libboost-all-dev build-essential libssl-dev default-jre-headless pkg-config libsqlite3-dev
sudo apt-get install -y gcovr lcov
sudo useradd -m jenkins
echo "jenkins ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/jenkins
sudo mkdir /home/jenkins/.ssh
sudo echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDHfJER+eGjDa0PCjN9V+VSYjMO9CiCkuTEi7w0Fio85PL/qBI67L3HmUkIFsIcA5fLiheClfeBvQPhojPpPrJSUD/3/iDgqLW/dWTQ64bpInuOCZJtZbSkVnbeVVyAxO7CF/0yh7W0NOg+IallnCimOEsDGhma686FYcvvOweCJ+w3bYA9fPwNGTBxsc++hTs/mR/WYehc1gECVZ1zE2EBNF+N9Uov+p+SuFkccpCVFM5tVzduNuQsWy6TRjoUK/q/P4BxhV8M/E2lbWK6SZ+ieOgQsC7UxBel5a6B7G//MqO4ZfHJEtpy5LuiuZMxjl/pdbfWlIwshjtnplH4BN35 ltr120@yi-jenkins" > /home/jenkins/.ssh/authorized_keys
sudo chown -R jenkins:jenkins /home/jenkins/.ssh
sudo chmod 700 /home/jenkins/.ssh
sudo chmod 600 /home/jenkins/.ssh/authorized_keys
EOF

Vagrant.configure("2") do |config|
  [32001, 32002].each do |port|
    config.vm.define "ubuntu-1604-64bit-#{port}" do |agent|
      agent.vm.box = "ndn-jenkins/ubuntu1604-amd64"
      agent.vm.hostname = "ubuntu-1604-64bit-#{port}"
      agent.vm.network "forwarded_port", guest: 22, host: port, id: "ssh"

      agent.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
        vb.cpus = "2"
        vb.customize ["storagectl", :id, "--name", "SATA Controller", "--hostiocache", "on"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end

      # Workaround for "stdin is not a tty"
      # see Vagrant issue #1673
      config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

      config.vm.provision "shell", inline: $setup_ubuntu
    end
  end

  [32011].each do |port|
    config.vm.define "ubuntu-1604-32bit-#{port}" do |agent|
      agent.vm.box = "ubuntu/xenial32"
      agent.vm.hostname = "ubuntu-1604-32bit-#{port}"
      agent.vm.network "forwarded_port", guest: 22, host: port, id: "ssh"

      agent.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
        vb.cpus = "1"
        vb.customize ["storagectl", :id, "--name", "SCSI", "--hostiocache", "on"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end

      # Workaround for "stdin is not a tty"
      # see Vagrant issue #1673
      config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

      config.vm.provision "shell", inline: $setup_ubuntu
    end
  end

  [32021].each do |port|
    config.vm.define "ubuntu-1804-64bit-#{port}" do |agent|
      agent.vm.box = "ndn-jenkins/ubuntu1804-amd64"
      agent.vm.hostname = "ubuntu-1804-32bit-#{port}"
      agent.vm.network "forwarded_port", guest: 22, host: port, id: "ssh"

      agent.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
        vb.cpus = "1"
        vb.customize ["storagectl", :id, "--name", "SATA Controller", "--hostiocache", "on"]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end

      # Workaround for "stdin is not a tty"
      # see Vagrant issue #1673
      config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

      config.vm.provision "shell", inline: $setup_ubuntu
    end
  end
end