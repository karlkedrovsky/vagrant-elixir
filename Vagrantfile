# -*- mode: ruby -*-
# vi: set ft=ruby :

module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end

Vagrant.configure("2") do |config|

  config.vm.box = "geerlingguy/ubuntu1604"
  
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  config.vm.hostname = "elixir"
  config.vm.network :private_network, ip: "10.1.0.16"
  # Assuming we're running on a mac or a linux host machine
  if OS.mac?
    config.vm.synced_folder "/Users/kkedrovsky/projects", "/projects", type: "nfs", nfs_export: false
  else
    config.vm.synced_folder "/projects", "/projects", type: "nfs", nfs_version: 4, nfs_udp: false, nfs_export: false
  end

  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.cpus = 1
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.inventory_path = "provisioning/inventory"
    ansible.limit = "all"
  end

end
