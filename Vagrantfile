# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'vmware_fusion'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  #config.vm.box = "ferventcoder/win2008r2-x64-nocm"
  config.vm.box = "opentable/win-2012r2-standard-amd64-nocm"
  config.vm.hostname = "sitecore8-dev"

  # windows config
  config.vm.guest = :windows
  config.windows.halt_timeout = 20
  config.vm.communicator = "winrm"
  config.winrm.username = "vagrant"
  config.winrm.password = "vagrant"
  #config.winrm.timeout = 300

  # network
  config.vm.network "private_network", ip: "192.168.30.10"

  # rdp
  config.vm.network :forwarded_port, guest: 3389, host: 1234

  # winrm
  config.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true

  # configure chef
  # vagrant plugin install vagrant-omnibus
  config.omnibus.chef_version = :latest

  # berkshelf
  # vagrant plugin install vagrant-berkshelf

  # Provision
  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.file_cache_path = "c:/chef/cache"
    chef.file_backup_path = "c:/chef/backup"
    chef.add_recipe "windows::default"
    chef.add_recipe "minitest-handler::default"
    chef.add_recipe "windows::reboot_handler"
    chef.add_recipe "chocolatey"
    chef.add_recipe "chocolatey-installer::default"
    chef.json = {
      "chocolatey-installer"=>{
        "packages"=> [
          'git',
          'notepadplusplus',
          'nodejs',
          'visualstudiocode',
          'GoogleChrome',
          'firefox',
          'atom',
          '7zip',
          'putty',
          'phantomjs'
        ]
      },
      "windows"=>{
        "reboot_timeout" => 15
      }
    }
  end

  config.vm.provision :reload

  # Provision
  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.file_cache_path = "c:/chef/cache"
    chef.file_backup_path = "c:/chef/backup"
    chef.add_recipe "chocolatey-installer::default"
    chef.json = {
      "chocolatey-installer"=>{
        "packages"=> [
          'mssqlserver2012express',
          'visualstudiocommunity2013'
        ]
      },
      "windows"=>{
        "reboot_timeout" => 15
      }
    }
  end

  config.vm.provision :reload

  # Provision
  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.file_cache_path = "c:/chef/cache"
    chef.file_backup_path = "c:/chef/backup"
    chef.add_recipe "chocolatey-installer::default"
    chef.json = {
      "chocolatey-installer"=>{
        "packages"=> [
          'dotnet4.5.1'
        ]
      },
      "windows"=>{
        "reboot_timeout" => 15
      }
    }
  end

  config.vm.provider :vmware_fusion do |v, override|
    v.gui = true
    v.vmx["memsize"] = "2048"
  end

end
