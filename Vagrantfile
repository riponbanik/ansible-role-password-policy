# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.ssh.insert_key = false

  config.vm.define :centos7 do |v| 	  
	  config.vm.provider "hyperv"
	  config.vm.network "public_network"
	  # config.vm.synced_folder ".", "/vagrant", disabled: true
	  
    config.vm.provider "hyperv" do |h|
	      h.cpus = 2
	      h.memory = 512
	      h.maxmemory = 1024
		  h.enable_virtualization_extensions = true
		  h.differencing_disk = false
	  end      
  
	   # Ansible
	  config.vm.provision :ansible_local do |ansible|
	    ansible.playbook       = "tests/test_vagrant.yml"
	    ansible.verbose        = true
	    ansible.install        = true
	    ansible.install_mode = "pip"
	    ansible.version = "2.4.3.0"
	    ansible.become = true                
	    ansible.limit          = "all" # or only "nodes" group, etc.
	    ansible.inventory_path = "tests/inventory_vagrant.ini"
	  end 

  end  
end