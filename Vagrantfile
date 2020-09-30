# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "bootstrap.sh"

  # K8S Master Node
  config.vm.define "kube-master" do |masternode|
    masternode.vm.box = "centos/7"
    masternode.vm.hostname = "kube-master.lab.com"
  #HostOnlyAdapter
    masternode.vm.network "private_network", ip: "10.10.10.10" 
    masternode.vm.provider "virtualbox" do |vm|
      vm.name = "kube-master"
      vm.memory = 2048
      vm.cpus = 2
      # Grouping VM
      vm.customize ["modifyvm", :id, "--groups", "/kubernetes"]
      # Add promicuous mode to all VMs
      vm.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
    end
    masternode.vm.provision "shell", path: "bootstrap_kubemaster.sh"
  end

  NodeCount = 2

  # K8S Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.define "kube-worker#{i}" do |workernode|
      workernode.vm.box = "centos/7"
      workernode.vm.hostname = "kube-worker#{i}.lab.com"
      workernode.vm.network "private_network", ip: "10.10.10.1#{i}"
      workernode.vm.provider "virtualbox" do |vm|
        vm.name = "kube-worker#{i}"
        vm.memory = 2048
        vm.cpus = 1
        # Prevent VirtualBox from interfering with host audio stack
        vm.customize ["modifyvm", :id, "--groups", "/kubernetes"]
      # Add promicuous mode to all VMs
      vm.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      end
      workernode.vm.provision "shell", path: "bootstrap_kubeworker.sh"
    end
  end

end
