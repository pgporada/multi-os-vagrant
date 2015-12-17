# -*- mode: ruby -*-
# vi: set ft=ruby :

# Changing the *_COUNT will create N number of servers.
LINUX_COUNT = 0
WINDOWS_COUNT = 0

Vagrant.configure("2") do |config|
    if LINUX_COUNT > 0 then
        (1..LINUX_COUNT).each do |linux_id|
            config.vm.define "centos7-linux#{linux_id}" do |linux|
				linux.vm.provider :virtualbox do |vb|
					vb.memory = 256
					vb.cpus = 1
					vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
					vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
					vb.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000 ]
				end
				linux.vm.box = "centos/7"
				linux.vm.network "private_network", ip: "192.168.82.#{linux_id + 50}"
				linux.ssh.username = "vagrant"
				linux.ssh.password = "vagrant"
			end
		end
	end

    if WINDOWS_COUNT > 0 then
        (1..WINDOWS_COUNT).each do |windows_id|
            config.vm.define "windows7-windows#{windows_id}" do |windows|
                windows.vm.communicator = "winrm"
                windows.vm.guest = :windows
				
				# Disable the vagrant-vbguest plugin as it doesn't detect Windows
				if Vagrant.has_plugin?("vagrant-vbguest")
					windows.vbguest.auto_update = false
				end
                
                windows.vm.provider :virtualbox do |vb|
                    vb.memory = 512
                    vb.cpus = 1
                    vb.gui = true
                    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
                    vb.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000 ]
                end

                windows.vm.box = "ferventcoder/win7pro-x64-nocm-lite"
                windows.vm.network "private_network", ip: "192.168.82.#{windows_id + 100}"
                windows.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true
                
                # Allow ICMP traffic
                windows.vm.provision "shell", inline: "netsh advfirewall firewall add rule name='All ICMP V4' dir=in action=allow protocol=icmpv4"
            end
        end
    end

end
