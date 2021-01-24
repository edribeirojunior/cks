# -*- mode: ruby -*-
# vi: set ft=ruby :
VM_ID = "ckstest"

IP_NW = "192.168.56."
MASTER_IP_START = 20
WORKER_IP_START = 30

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = true

# Single Control-Plane node  
  config.vm.define "#{VM_ID}-master" do |node|
      node.vm.provider "virtualbox" do |vb|
          vb.name = "#{VM_ID}-master1"
          vb.memory = 2048
          vb.cpus = 2
      end
      node.vm.hostname = "#{VM_ID}-master"
      node.vm.network :private_network, ip: IP_NW + "#{MASTER_IP_START}"

      node.vm.provision "setup-hosts", :type => "shell", :path => "scripts/setup-hosts.sh" do |s|
        s.args = ["enp0s8"]
      end
      node.vm.provision "setup-dns", type: "shell", :path => "scripts/update-dns.sh"
      node.vm.provision "init-k8s", type: "shell", :path => "scripts/install_master.sh" do |s|
        s.args = ["enp0s8"]
      end
  end

# Worker nodes /docker
  config.vm.define "#{VM_ID}-worker" do |node|
    node.vm.provider "virtualbox" do |vb|
        vb.name = "#{VM_ID}-worker"
        vb.memory = 2048
        vb.cpus = 2
    end
    node.vm.hostname = "#{VM_ID}-worker"
    node.vm.network :private_network, ip: IP_NW + "#{WORKER_IP_START}"

    node.vm.provision "setup-hosts", :type => "shell", :path => "scripts/setup-hosts.sh" do |s|
      s.args = ["enp0s8"]
    end
    
    node.vm.provision "setup-dns", type: "shell", :path => "scripts/update-dns.sh"
    node.vm.provision "install_worker", type: "shell", :path => "scripts/install_worker.sh"
    node.vm.provision "join-cluster", type: "shell", :path => "scripts/kubeadm-join.sh"
  end

# Worker nodes /containerd
  config.vm.define "#{VM_ID}-worker2" do |node|
    node.vm.provider "virtualbox" do |vb|
        vb.name = "#{VM_ID}-worker2"
        vb.memory = 2048
        vb.cpus = 2
    end
    node.vm.hostname = "#{VM_ID}-worker2"
    node.vm.network :private_network, ip: IP_NW + "#{WORKER_IP_START+1}"

    node.vm.provision "setup-hosts", :type => "shell", :path => "scripts/setup-hosts.sh" do |s|
      s.args = ["enp0s8"]
    end
    
    node.vm.provision "setup-dns", type: "shell", :path => "scripts/update-dns.sh"
    node.vm.provision "install_worker", type: "shell", :path => "scripts/install_worker.sh"
    node.vm.provision "join-cluster", type: "shell", :path => "scripts/kubeadm-join.sh"
    node.vm.provision "containerd", type: "shell", :path => "scripts/install_worker_containerd.sh"
  end
end
