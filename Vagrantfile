# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/bionic64"
  # Hardening box version
  config.vm.box_version = "20200325.0.0"

  config.ssh.insert_key = true
  # create management node. This node will run
  config.vm.define :mgmt do |mgmt_config|
    mgmt_config.vm.hostname = "management"
    mgmt_config.vm.network :private_network, ip: "38.19.93.10"
    mgmt_config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
    mgmt_config.vm.provision :shell, path: "provisioning/shell/boostrap-management-node.sh"
  end

  # Create master server 1
  config.vm.define :server1 do |server1_config|
    server1_config.vm.hostname = "server1"
    server1_config.vm.network :private_network, ip: "38.19.93.11"
    server1_config.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
    end
  end

  # Create master server 2
  config.vm.define :server2 do |server2_config|
    server2_config.vm.hostname = "server2"
    server2_config.vm.network :private_network, ip: "38.19.93.12"
    server2_config.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
    end
  end
  # create Consul/Nomad agent node.
  (1..1).each do |i|
    config.vm.define "agent#{i}" do |agent_config|
      agent_config.vm.hostname = "agent#{i}"
      agent_config.vm.network :private_network, ip: "38.19.93.2#{i}"
      agent_config.vm.provider "virtualbox" do |vb|
        if i == 1
          vb.memory = "1024" # Can custom memory here
        else
          vb.memory = "1024"
        end
      end
    end
  end

  config.vm.provision :shell, :inline => "sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config && systemctl reload sshd"
end
