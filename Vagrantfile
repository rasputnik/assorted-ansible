# -*- mode: ruby -*-
# vi: set ft=ruby :

## if you change these, change vagrant/hosts too
#
# would _dearly_ love to make this less clunkable,
# and just read vagrant/hosts directly.

hosts = [
  {:name => "nexus1",  :ip => "10.144.0.4", :ram => 2048},
  {:name => "gitlab1", :ip => "10.144.0.8", :ram => 2048},
]

Vagrant.configure("2") do |config|

  # setup /etc/hosts on each vm
  # - requires the vagrant-hostmanager plugin
  config.hostmanager.enabled = true
  # set to 'false' if you don't want to manage _your_ /etc/hosts
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true

  hosts.each do |host|
    config.vm.define host[:name] do |c|

      c.vm.box = "box-cutter/centos68"

      # stop Vagrant 'helping'
      c.ssh.insert_key = false

      c.vm.hostname = host[:name]

      c.vm.network :private_network, ip: host[:ip], netmask: "255.255.255.0"

      c.vm.provider("virtualbox") do |vb|
        vb.memory = host[:ram]
      end

      # turn off shared folder
      c.vm.synced_folder ".", "/vagrant", :disabled => true
    end
  end
end
