# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise32"
  config.vm.host_name = "sylius"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  config.vm.network :hostonly, "172.33.33.34"
  config.vm.share_folder "vagrant-root", "/var/www", "./www" , :nfs => true

  # Whithout this symlinks can't be created on the shared folder.
  config.vm.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]

  # Chef solo configuration.
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "./"
    # Chef debug level, start vagrant like this to debug:
    # $ CHEF_LOG_LEVEL=debug vagrant <provision or up>
    chef.log_level = ENV['CHEF_LOG'] || "info"

    chef.cookbooks_path = "cookbooks"
    chef.add_recipe("apt")
    chef.add_recipe("sylius")

    chef.json = {
          "mysql" => {
            "server_root_password" => "iloverandompasswordsbutthiswilldo",
            "server_repl_password" => "iloverandompasswordsbutthiswilldo",
            "server_debian_password" => "iloverandompasswordsbutthiswilldo"
          }
    }
  end

  if File.exist? "./Vagrantfile.local"
    load "./Vagrantfile.local"
  end

end
