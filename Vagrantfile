# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

@ansible_home = "/vagrant"
boxes = {
  "centos-7" => {
                        :name => "centos/7",
                        :cpu => "1",
                        :ram => "512"
                      },
  "centos-6" => {
                        :name => "centos/6",
                        :cpu => "1",
                        :ram => "512"
                  },
  "ubuntu-xenial" => {
                        :name => "ubuntu/xenial64",
                        :cpu => "1",
                        :ram => "512"
                    },
  "ubuntu-trusty" => {
                        :name => "ubuntu/trusty64",
                        :cpu => "1",
                        :ram => "512"
                      }
}



Vagrant.configure("2") do |config|
  boxes.each do |box_name, box|

    config.vm.define box_name do |machine|
       machine.vm.box = box[:name]
       machine.vm.hostname = "%s" % box_name
       machine.ssh.insert_key = false
    	 machine.vm.provider "hyperv"
    	 machine.vm.network "public_network"

       # Hyperv Provider
        machine.vm.provider "hyperv" do |h|
    	      h.cpus = 1
    	      h.memory = 512
    	      h.maxmemory = 1024
    		  h.enable_virtualization_extensions = true
    		  h.differencing_disk = false
          machine.vm.synced_folder ".", "/vagrant", mount_options: ["vers=3.0"]
          # Copy the Ansible playbook over to the guest machine, run rsync-auto to automatically
          # pull in the latest changes while a VM is running.
          # config.vm.synced_folder "../", "#{@ansible_home}/roles/", type: 'rsync'
    	  end

        # Shell
        machine.vm.provision "shell", inline: <<-SHELL
          #/bin/sh -c 'which yum' && $(sudo yum -y install epel-release; sudo yum -y install python-pip) || true
          #/bin/sh -c 'which apt-get' && $(sudo apt-get -y install python-pip) || true
          sudo pip install -U setuptools || true
        SHELL

        # Virtualbox Provider
        machine.vm.provider :virtualbox do |vb|
           vb.memory = 512
           vb.cpus = 1
           vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
           vb.customize ["modifyvm", :id, "--ioapic", "on"]
           machine.vm.synced_folder ".", "/vagrant"
        end

    	# Ansible
    	machine.vm.provision :ansible_local do |ansible|
    	   ansible.playbook       = "tests/test_vagrant.yml"
    	   ansible.verbose        = "vvv"
    	   ansible.install        = true
    	   ansible.install_mode = "pip"
    	   ansible.version = "2.4.3.0"
    	   ansible.become = true
    	   ansible.limit          = "all" # or only "nodes" group, etc.
    	   ansible.inventory_path = "tests/inventory_vagrant.ini"
    	end

    end
  end
end
