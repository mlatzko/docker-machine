# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Name of the machine
BOX_NAME = "docker-machine"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # vagrant box used

  config.vm.box = "ubuntu/trusty64"
  #config.vm.box_url = "http://files.vagrantup.com/trusty64.box"

  # host name
  config.vm.host_name = "#{BOX_NAME}.localdomain"

  config.vm.network "private_network", ip: "10.2.0.10", netmask: "255.255.0.0"

  # forward ports
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

  # cache folder
  config.vm.synced_folder "aptcache", "/var/cache/apt/archives/", :mount_options => ["uid=33", "dmode=777", "fmode=777"]

  # central log folder
  config.vm.synced_folder "logs", "/var/log/vagrant", :mount_options => ["uid=33", "dmode=777", "fmode=777"]

  # mounted projects
  config.vm.synced_folder "../angularjs-example", "/srv/angularjs-example", :mount_options => ["uid=33", "dmode=777", "fmode=777"]

  # hardware settings
  config.vm.provider "virtualbox" do |vb|
    # Don't boot with headless mode
    vb.gui = false

    # Set the name that appears in the VirtualBox GUI
    vb.name = BOX_NAME

    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "4096","--cpus", "4"]
  end

  # Docker provisioning
  config.vm.provision "docker" do |d|
    d.build_image "https://github.com/mlatzko/docker-angularjs-container.git", 
      args: '-t docker-angularjs-container'

    d.run "angularjs.example",
      image: "docker-angularjs-container", 
      args:  "-p 80:80 -v '/srv/angularjs-example:/srv/www'",
      daemonize: true
  end
end
