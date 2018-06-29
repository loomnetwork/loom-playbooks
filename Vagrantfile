# -*- mode: ruby -*-
# vi: set ft=ruby :

# Size of the cluster created by Vagrant
num_instances = 4

# Private Network Prefix
private_network_prefix = "172.31.99."

# Build numbers
loom_build = "latest"

# Hosts
host_vars = {}

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provision "shell",
    inline: "apt-get update && apt-get install -y python"

  (0..num_instances-1).each do |i|
    vm_name = "%s-%d" % ["loom", i]
    vm_ip = "#{private_network_prefix}#{i+10}"
    config.vm.define vm_name do |node|
      node.vm.hostname = vm_name
      node.vm.network "private_network", ip: vm_ip
      host_vars[vm_name] = {"private_ip" => vm_ip}
      node.vm.provider :virtualbox do |vb|
        vb.memory = "1536"
        vb.cpus = "1"
        vb.name = vm_name
      end
      # Run only after all machines are up
      if i == num_instances-1
        node.vm.provision "ansible" do |ansible|
          ansible.playbook = "loom-playbook.yml"
          ansible.limit = "all,localhost"
          ansible.verbose = "-vv"
          ansible.host_vars = host_vars
          ansible.extra_vars = {
            user: "vagrant",
            loom_build: loom_build,
            working_directory: "/home/vagrant"
          }
        end
      end
    end
  end
end
