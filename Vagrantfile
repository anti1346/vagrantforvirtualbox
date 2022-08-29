# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  $vm_provider = "virtualbox"
  $box_image = "ubuntu/focal64"

  $vm_name_prefix = "q0zusnir"

  $number_of_control_planes = 1
  $number_of_nodes = 1

  $vm_subnet = "192.168.56"

  $vm_control_plane_cpus = 2
  $vm_control_plane_memory = 2048
  $vm_node_cpus = 2
  $vm_node_memory = 2048

  # Controllers
  (1..$number_of_control_planes).each do |i|
    config.vm.define "#{$vm_name_prefix}-control#{i}" do |node|
      node.vm.box = $box_image
      node.vm.provider $vm_provider do |vm|
        vm.name = "#{$vm_name_prefix}-control#{i}"
        vm.cpus = $vm_control_plane_cpus
        vm.memory = $vm_control_plane_memory
      end
      node.vm.hostname = "#{$vm_name_prefix}-control#{i}"
      node.vm.network "private_network", ip: "#{$vm_subnet}.1#{i}", nic_type: "virtio"
    end
  end

  # Nodes
  (1..$number_of_nodes).each do |i|
    config.vm.define "#{$vm_name_prefix}-node#{i}" do |node|
      node.vm.box = $box_image
      node.vm.provider $vm_provider do |vm|
        vm.name = "#{$vm_name_prefix}-node#{i}"
        vm.cpus = $vm_node_cpus
        vm.memory = $vm_node_memory
      end
      node.vm.hostname = "#{$vm_name_prefix}-node#{i}"
      node.vm.network "private_network", ip: "#{$vm_subnet}.2#{i}", nic_type: "virtio"
    end
  end

  # CentOS
  config.vm.define "#{$vm_name_prefix}-node3" do |node|
    node.vm.box = "generic/centos8"
    node.vm.provider "virtualbox" do |vm|
      vm.name = "#{$vm_name_prefix}-node3"
      vm.cpus = $vm_node_cpus
      vm.memory = $vm_node_memory
    end
    node.vm.hostname = "#{$vm_name_prefix}-node3"
    node.vm.network "private_network", ip: "192.168.56.23", nic_type: "virtio"
  end
  
  # Disable Synced Folder
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Enable SSH Password Authentication
  config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
    sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
    sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart ssh
  SHELL
end
