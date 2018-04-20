# -*- mode: ruby -*-
# vi: set ft=ruby :

raise "vagrant-libvirt plugin must be installed, try $ vagrant plugin install vagrant-libvirt" unless Vagrant.has_plugin? "vagrant-libvirt"

Vagrant.configure("2") do |config|


  config.vm.define "ansiblesetup" do |device|
    device.vm.hostname = "ansiblesetup"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 512
    end

    # NETWORK INTERFACES
    device.vm.network "private_network",
      :ip => '10.1.1.10',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net'
    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'ansiblesetup.yml'
    end

  end


  config.vm.define "tower1" do |device|
    device.vm.hostname = "tower1"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 98304
    end

    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
    end

    # NETWORK INTERFACES
    device.vm.network "private_network",
      :ip => '10.1.1.4',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net'
  end

  config.vm.define "postgres1" do |device|
    device.vm.hostname = "postgres1"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 1024
    end
    device.vm.network "private_network",
      :ip => '10.1.1.3',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net'
    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
    end


  end

end
