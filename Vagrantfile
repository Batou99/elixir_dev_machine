# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_plugin "vagrant-berkshelf"
Vagrant.require_plugin "vagrant-omnibus"

Vagrant.configure("2") do |config|
  config.vm.hostname = "elixir-berkshelf"

  # Set the version of chef to install using the vagrant-omnibus plugin
  config.omnibus.chef_version = :latest

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "opscode_ubuntu-12.04_provisionerless"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_provisionerless.box"

  config.vm.network :private_network, ip: "33.33.33.10"

  config.vm.provider :virtualbox do |box|
    box.customize ["modifyvm", :id, "--cpus", "2"]
    box.customize ["modifyvm", :id, "--memory", "2048"]
  end

  config.vm.synced_folder "src/", "/home/vagrant/src"
  config.ssh.forward_agent = true

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "elixir"
    chef.add_recipe "console-development"
    chef.json.merge! rvm: {vagrant: {system_chef_solo: '/usr/bin/chef-solo'}}
  end

  config.vm.provision "shell", inline: "echo 'export PATH=$PATH:/usr/local/lib/elixir/bin' >> /home/vagrant/.zshrc"
    
end
