# -*- mode: ruby -*-
# vi: set ft=ruby :

# Changing the NODE_COUNT will create N node servers.
NODE_COUNT = 2

Vagrant.configure("2") do |config|

    # Disable the vagrant-vbguest plugin as it doesn't detect Windows
    if Vagrant.has_plugin?("vagrant-vbguest")
        config.vbguest.auto_update = false
    end

    if NODE_COUNT > 0 then
        (1..NODE_COUNT).each do |node_id|
            config.vm.define "windows7-node#{node_id}" do |node|
                node.vm.communicator = "winrm"
                node.vm.guest = :windows
                
                node.vm.provider :virtualbox do |vb|
                    vb.memory = 512
                    vb.cpus = 1
                    vb.gui = true
                    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
                    vb.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000 ]
                end

                node.vm.box = "ferventcoder/win7pro-x64-nocm-lite"
                node.vm.network "private_network", ip: "192.168.82.#{node_id + 100}"
                node.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true
                
                # Allow ICMP traffic
                node.vm.provision "shell", inline: "netsh advfirewall firewall add rule name='All ICMP V4' dir=in action=allow protocol=icmpv4"
            end
        end
    end

end
