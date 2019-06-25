# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
# REQUIRED_PLUGINS        = %w(vagrant-vbguest vagrant-librarian-chef-nochef vagrant-disksize)
REQUIRED_PLUGINS        = %w(vagrant-vbguest vagrant-disksize)

plugins_to_install = REQUIRED_PLUGINS.select { |plugin| not Vagrant.has_plugin? plugin }
if not plugins_to_install.empty?
  puts "Installing required plugins: #{plugins_to_install.join(' ')}"
  if system "vagrant plugin install #{plugins_to_install.join(' ')}"
    exec "vagrant #{ARGV.join(' ')}"
  else
    abort "Installation of one or more plugins has failed. Aborting. Please read the Bike Index README."
  end
end

PORTS = [
  # rails
  { guest: 3000, host: 4000},
  # pgadmin
  { guest: 5050, host: 5050},
  # pg
  { guest: 5432, host: 5432},
  # rabbitmq
  { guest: 5672, host: 5672},
  # redis
  { guest: 6379, host: 6379},
  # mattermost
  { guest: 8065, host: 8065},
  # rabbitmq management
  { guest: 15672, host: 15672}
]

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"

  # Customize the amount of disk on the VM:
  # TODO: vagrant plugin install vagrant-disksize
  config.disksize.size = '60GB'

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  PORTS.each do |port|
    config.vm.network "forwarded_port", guest: port[:guest], host: port[:host]
  end

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

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  config.vm.synced_folder "../wsl", "/vagrant"

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
  config.vm.provider "virtualbox" do |vb|
    vb.name = "ubuntu"

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
    # vb.memory = "4096"
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  # Use Chef Solo to provision our virtual machine
  # config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = ["cookbooks", "site-cookbooks"]

  #   chef.add_recipe "apt"
  #   chef.add_recipe "build-essential"
  #   chef.add_recipe "system::install_packages"
  #   chef.add_recipe "ruby_build"
  #   chef.add_recipe "ruby_rbenv::user"
  #   chef.add_recipe "ruby_rbenv::user_install"
  #   chef.add_recipe "vim"
  #   chef.add_recipe "postgresql::server"
  #   chef.add_recipe "postgresql::client"

  #   chef.json = {
  #     rbenv: {
  #       user_installs: [{
  #         user: 'ubuntu',
  #         rubies: ["2.4.0"],
  #         global: "2.4.0",
  #         gems: {
  #         "2.4.0" => [{ name: "bundler" }]
  #       }
  #       }]
  #     },
  #     system: {
  #       packages: {
  #         install: ["redis-server", "nodejs", "libpq-dev"]
  #       }
  #     },
  #     postgresql: {
  #       :pg_hba => [{
  #         :comment => "# Add vagrant role",
  #         :type => 'local', :db => 'all', :user => 'postgres', :addr => nil, :method => 'trust'
  #       }],
  #       :users => [{
  #         "username": "postgres",
  #         "password": "postgres",
  #         "superuser": true,
  #         "replication": false,
  #         "createdb": true,
  #         "createrole": false,
  #         "inherit": false,
  #         "login": true
  #       }]
  #     }
  #   }
  # end
end
