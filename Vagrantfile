# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # operating system for the VM
  config.vm.box = "ubuntu/xenial64"

  # ssh settings
  config.ssh.private_key_path = ["~/.ssh/ckalabvm", "~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false

  # upload public key into the machine
  config.vm.provision "file", source: "~/.ssh/ckalabvm.pub", destination: "~/.ssh/authorized_keys"

  # configure the master node
  config.vm.define "master" do |master|
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "172.28.128.3"
    master.vm.provision :ansible do |ansible|
      ansible.inventory_path = "configurations/hosts"
      ansible.verbose = "vvvv"
      ansible.raw_arguments  = ["--private-key=~/.ssh/ckalabvm"]
      ansible.playbook = "configurations/ckalab.yml"
    end
  end 
  
  # synchronize folders
  config.vm.synced_folder ".", "/home/vagrant/ckalab",disabled: true

  # vm provider
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    #vb.gui = true   

    # Correct this error Stderr: VBoxManage.exe: error: RawFile#0 failed to create the raw output 
    vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]

    # Group the Machines
    vb.customize ["modifyvm", :id, "--groups", "/Kubernetes Cluster"]

    # Resize Hard Disk
    #vb.customize ["modifymedium", "--resize", "12288"]

    # Customize the number CPUS on the VM:
    vb.cpus = "2"

    # Customize the amount of memory on the VM:
    vb.memory = "1280"
  end
end
