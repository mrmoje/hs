# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.vm.box = "ubuntu/trusty64"

    disk1 = '.vagrant/disk1.vdi'
    disk2 = '.vagrant/disk2.vdi'

    config.vm.define "hstack" do |machine|
        machine.vm.network :private_network, ip: "10.1.0.5",
            :netmask => "255.255.0.0"
        machine.vm.network :private_network, ip: "10.0.0.11",
            :netmask => "255.255.0.0"
        machine.vm.network :private_network, ip: "10.0.0.41",
            :netmask => "255.255.0.0"
        machine.vm.network :private_network, ip: "10.0.0.51",
            :netmask => "255.255.0.0"
        machine.vm.hostname = "hstack"
        machine.vm.provider :virtualbox do |v, o|
            o.vm.provision "shell", inline: "sudo apt-get update; sudo apt-get --yes install avahi-daemon"
            o.ssh.insert_key = false
            o.vm.hostname = "hstack.local"
            v.customize ["modifyvm", :id, "--memory", 2048]
            v.customize ["createhd", "--filename", disk1, "--size", 1024]
            v.customize ["createhd", "--filename", disk2, "--size", 1024]
            v.customize ["storageattach", :id, "--storagectl", "SATAController",
                         "--port", 1, "--device", 0, "--type", "hdd",
                         "--medium", disk1]
            v.customize ["storageattach", :id, "--storagectl", "SATAController",
                         "--port", 2, "--device", 0, "--type", "hdd",
                         "--medium", disk2]
        end
    end
end
