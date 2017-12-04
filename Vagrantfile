# -*- mode: ruby -*-
# vi: set ft=ruby :

raise "vagrant-libvirt plugin must be installed, try $ vagrant plugin install vagrant-libvirt" unless Vagrant.has_plugin? "vagrant-libvirt"

postgresnodes = {
  :postgres1 => {
    :ipaddr => "10.1.1.3",
    :bridge  => "vault_net",
    :group => 'tower1'
  },
  :postgres2 => {
    :ipaddr => "10.1.4.3",
    :bridge => "vault_net2",
    :group => 'tower2'
  }
}

towernodes = {
  "tower1a" => {
    :ipaddr => "10.1.1.4",
    :bridge => "vault_net",
    :group => 'tower1'
  },
  "tower1b" => {
    :ipaddr => "10.1.1.5",
    :bridge => "vault_net",
    :group => 'tower1'
  },
  "tower1c" => {
    :ipaddr => "10.1.1.6",
    :bridge => "vault_net",
    :group => 'tower1'
  },
  "tower2a" => {
    :ipaddr => "10.1.4.10",
    :bridge => "vault_net2",
    :group => 'tower2'
  },
  "tower2b" => {
    :ipaddr => "10.1.4.11",
    :bridge => "vault_net2",
    :group => 'tower2'
  },
  "tower2c" => {
    :ipaddr => "10.1.4.12",
    :bridge => "vault_net2",
    :group => 'tower2'
  },
  "tower3a" => {
    :ipaddr => "10.1.3.10",
    :bridge => "vault_net3",
    :group => 'tower3'
  },
  "tower3b" => {
    :ipaddr => "10.1.3.11",
    :bridge => "vault_net3",
    :group => 'tower3'
  },
  "tower4a" => {
    :ipaddr => '10.1.5.3',
    :bridge => 'vault_net5',
    :group => 'tower4'
  }
}

testservers = {
  "tower1test" => {
    :ipaddr => '10.1.1.20',
    :bridge => 'vault_net'
  },
  "tower2test" => {
    :ipaddr => '10.1.4.20',
    :bridge => 'vault_net2'
  },
  "tower3test" => {
    :ipaddr => '10.1.3.20',
    :bridge => 'vault_net3'
  },
  "tower4test" => {
    :ipaddr => '10.1.5.20',
    :bridge => 'vault_net5'
  }
}

allservers = postgresnodes.merge(towernodes)
allservers = allservers.merge(testservers)

vm_box = 'centos7'
ansible_play = 'centos.yml'
default_mem = 2048

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
      ansible.extra_vars = {
        ruby_etc_hosts: allservers,
        tower_servers: towernodes.keys(),
        postgres_servers: postgresnodes.keys()
      }
    end

  end

  allservers.each do |nodename, values|
    config.vm.define "#{nodename}" do |device|
      device.vm.hostname = nodename
      device.vm.box = vm_box
      device.vm.synced_folder '.' '/vagrant', :disabled => true
      device.vm.provider :libvirt do |v|
        v.memory = default_mem
      end
      device.vm.provision :ansible do |ansible|
        ansible.playbook = ansible_play
        ansible.extra_vars = {
          ruby_etc_hosts: allservers
        }
        if values[:group]
          ansible.groups = {
            "#{values[:group]}": [nodename]
          }
        end
      end

      # NETWORK INTERFACES
      device.vm.network "private_network",
        :ip => values[:ipaddr],
        :prefix => '24',
        :libvirt__forward_mode => 'veryisolated',
        :libvirt__dhcp_enabled => false,
        :libvirt__network_name => values[:bridge]
    end
  end

  config.vm.define "fw1" do |device|
    device.vm.hostname = "fw1"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 512
    end

    device.vm.network "private_network",
      :ip => '10.1.1.2',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net'

    device.vm.network "private_network",
      :ip => '10.1.4.2',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net2'

    device.vm.network "private_network",
      :ip => '10.1.2.2',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net4'
    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
      ansible.extra_vars = {
          ruby_etc_hosts: allservers
        }

    end

  end


  config.vm.define "fw2" do |device|
    device.vm.hostname = "fw2"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 512
    end

    device.vm.network "private_network",
      :ip => '10.1.3.2',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net3'
    device.vm.network "private_network",
      :ip => '10.1.2.3',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net4'
    device.vm.network "private_network",
      :ip => '10.1.5.2',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net5'

    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
       ansible.extra_vars = {
          ruby_etc_hosts: allservers
        }

    end
  end
end
