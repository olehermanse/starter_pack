# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    # Use a custom box:
    # https://scotch.io/tutorials/how-to-create-a-vagrant-base-box-from-an-existing-one
    config.vm.box = "basebox"
    # config.vm.box = "ubuntu/trusty64"

    # Run bootstrap.sh script on first boot:
    config.vm.provision "bootstrap", type: "shell", path: "bootstrap.sh"
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    config.vm.synced_folder "~/code/northern.tech", "/northern.tech", type: "virtualbox"

    # Performace settings for each vm:
    config.vm.provider "virtualbox" do |vb|
        vb.memory = 2048 # 1 GiB of memory
        vb.cpus = 2      # 2 CPU Cores

        # Ensure time synchronization:
        vb.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 1000 ]
    end

    # Main development machine: (No network)
    config.vm.define "dev", primary: true, autostart: false do |dev|
        config.vm.hostname = "DEV"
    end

    # ============================ BUILD MACHINES: ===========================

    config.vm.define "build", autostart: false do |build|
        config.vm.hostname = "build"
        config.vm.box = "buildbox"
        config.vm.provision "bootstrap", type: "shell", path: "buildbox/bootstrap.sh"
        config.vm.network "private_network", ip: "192.168.100.100"
    end

    # Dedicated mingw compile for windows machine:
    config.vm.define "mingw", primary: false, autostart: false do |mingw|
        config.vm.hostname = "mingw"
        config.vm.network "private_network", ip: "192.168.200.200"
    end

    # ============================ TEST MACHINES: ============================

    # Hub test machine:
    config.vm.define "hub", autostart: false do |hub|
        config.vm.hostname = "hub"
        config.vm.network "private_network", ip: "192.168.10.10"
    end

    # Client test machine:
    config.vm.define "client", autostart: false do |client|
        config.vm.hostname = "client"
        config.vm.network "private_network", ip: "192.168.10.11"
    end

    # =============================== BASE BOX: ==============================

    # Prepackage a box on disk:
    config.vm.define "basebox", autostart: false do |custombox|
        config.vm.box = "ubuntu/trusty64"
        config.vm.provision "bootstrap", type: "shell", path: "basebox/bootstrap.sh"
    end

    # Prepackage a box on disk:
    config.vm.define "buildbox", autostart: false do |buildbox|
        config.vm.box = "ubuntu/trusty64"
        config.vm.provision "bootstrap", type: "shell", path: "buildbox/bootstrap.sh"
    end
end
