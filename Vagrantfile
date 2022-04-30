# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
# Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
VAGRANT_EXPERIMENTAL="dependency_provisioners"
Vagrant.require_version ">=2.2.6"
Vagrant.configure("2") do |config|
  # config.vm.boot_timeout = 60
  # ssh config
  config.ssh.insert_key = true
  # config.ssh.compression = false
  config.ssh.connect_timeout = 20
  # config.ssh.port = 22
  # config.ssh.private_key_path = "id_rsa"
  # config.ssh.password = "vagrant"
  # config.ssh.username = "vagrant"
  

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  config.vm.provider "virtualbox" do |v|
    v.gui = false
    v.name = "Cyriaque"
    v.default_nic_type = "82540EM"
    v.memory = 2048
    v.cpus = 2
    v.linked_clone = true
  end
  config.vm.hostname = "trex.local"


  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/focal64"
  config.vm.box_version = "20220427.0.0"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false
  config.vm.box_download_checksum = "f8193d59dd7a8567c91c47f1cc5d6b4dac716ec05d8d2c66ae9d20a3f07c2f4f"
  config.vm.box_download_checksum_type = "sha256"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
  # network below

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder "share/", "/home/vagrant/shared", create: true, disable: false, group: "vagrant", owner: "vagrant", automount: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  config.vm.provision "OS updater", type: "shell" , run: "always", inline: <<-SHELL
    echo "Executing update and upgrade OS" && apt-get update && apt-get upgrade -y && echo "Done..."
  SHELL
  config.vm.provision "Package installer", type: "shell", run: "once", inline: <<-SHELL
    apt-get install -y traceroute ifupdown python3.8-venv sshuttle git  net-tools python3-pip
  SHELL
  config.vm.provision "Configure netplan cloud-init", type: "shell", run: "once", inline: <<-SHELL
    ls -1 /sys/class/net/ | grep -E "([^lo]|enp.{1,})" | xargs -I {} bash -c "sed -i 's/{}/eth0/g' /etc/netplan/50-cloud-init.yaml"
  SHELL
  config.vm.provision "GRUB editor", type: "shell", run: "once", inline: <<-SHELL
    sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"/g' /etc/default/grub
  SHELL
  config.vm.provision "Update GRUB", type: "shell", reboot: true, run: "once", inline: <<-SHELL
    update-grub
  SHELL
  config.vm.provision "Copying ssh public key", type: "file", source: "id_rsa.pub", run: "once", destination: "/home/vagrant/.ssh/id_rsa.pub"
  config.vm.provision "Copying ssh private key", type: "file", source: "id_rsa", run: "once", destination: "/home/vagrant/.ssh/id_rsa"
  config.vm.provision "Install ssh to authorizhed_keys", type: "shell", run: "once", inline: <<-SHELL
    cat /home/vagrant/.ssh/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
  SHELL
  config.vm.provision "Separator start", before: :each, type: "shell", run: "always", inline: <<-SHELL
    echo "\x1b[92;40m***************[START STEP]***************\x1b[0m"
  SHELL
  config.vm.provision "Separator end", after: :each, type: "shell", run: "always", inline: <<-SHELL
    echo "\x1b[92;40m***************[END STEP]***************\x1b[0m"
  SHELL
  # VBoxManage path - C:\"Program Files"\Oracle\VirtualBox\vboxmanage.exe
  # 1. vboxmanage.exe hostonlyif create
  # Output from command 1. 
  # 0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
  # Interface 'VirtualBox Host-Only Ethernet Adapter #2' was successfully created
  # Copy interface name for next step and command config.vm.network that below
  # 2. vboxmanage.exe hostonlyif ipconfig "VirtualBox Host-Only Ethernet Adapter #2" --ip 192.168.99.1 --netmask 255.255.255.252  
  config.vm.network "private_network", ip: "192.168.99.2", name: "VirtualBox Host-Only Ethernet Adapter #2"
  # Cahnge interface name 'enp' to 'eth'
  config.vm.provision "Configure netplan vagrant", after: :each, type: "shell", run: "once", inline: <<-SHELL
    sed -i 's/enp[0-9]s[0-9]/eth1/gm' /etc/netplan/50-vagrant.yaml && printf "      routes:\n      - to: default\n        via: 192.168.99.1\n        metric: 101\n      nameservers:\n        addresses: [1.1.1.1, 8.8.8.8]\n" >> /etc/netplan/50-vagrant.yaml && netplan apply
  SHELL
end
