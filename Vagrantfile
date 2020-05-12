# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.provision "shell", path: "bootstrap.sh"

  # K8S Master Node
  config.vm.define "kube-master" do |masternode|
    masterode.vm.box = "centos/7"
    masternode.vm.hostname = "kube-master.lab.com"
    masternode.vm.network "private_network", ip: "192.168.100.100" #HostOnlyAdapter
    masternode.vm.provider "virtualbox" do |vm|
      vm.name = "kube-master"
      vm.memory = 2048
      vm.cpus = 2
      # Prevent VirtualBox from interfering with host audio stack
      vm.customize ["modifyvm", :id, "--audio", "none"]
    end
    masternode.vm.provision "shell", path: "bootstrap_kubemaster.sh"
  end

  NodeCount = 3

  # K8S Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.define "kube-worker#{i}" do |workernode|
      workernode.vm.box = "centos/7"
      workernode.vm.hostname = "kube-worker#{i}.lab.com"
      workernode.vm.network "private_network", ip: "192.168.100.100#{i}"
      workernode.vm.provider "virtualbox" do |vm|
        vm.name = "kube-worker#{i}"
        vm.memory = 1024
        vm.cpus = 1
        # Prevent VirtualBox from interfering with host audio stack
        vm.customize ["modifyvm", :id, "--audio", "none"]
      end
      workernode.vm.provision "shell", path: "bootstrap_kubeworker.sh"
    end
  end

end
