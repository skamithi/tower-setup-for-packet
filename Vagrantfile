# Created by Topology-Converter v4.1.0
#    using topology data from: examples/1switch_1server.dot
#    NOTE: in order to use this Vagrantfile you will need:
#       -Vagrant(v1.8.1+) installed: http://www.vagrantup.com/downloads
#       -Cumulus Plugin for Vagrant installed: $ vagrant plugin install vagrant-cumulus
#       -the "helper_scripts" directory that comes packaged with topology-converter.py
#        -Libvirt Installed -- guide to come
#       -Vagrant-Libvirt Plugin installed: $ vagrant plugin install vagrant-libvirt
#       -Boxes which have been mutated to support Libvirt -- see guide below:
#            https://community.cumulusnetworks.com/cumulus/topics/converting-cumulus-vx-virtualbox-vagrant-box-gt-libvirt-vagrant-box
#       -Start with \"vagrant up --provider=libvirt --no-parallel\n")

raise "vagrant-libvirt plugin must be installed, try $ vagrant plugin install vagrant-libvirt" unless Vagrant.has_plugin? "vagrant-libvirt"
raise "vagrant-cumulus plugin must be installed, try $ vagrant plugin install vagrant-cumulus" unless Vagrant.has_plugin? "vagrant-cumulus"

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
      ansible.playbook = 'centos.yml'
    end

  end


  config.vm.define "tower1" do |device|
    device.vm.hostname = "tower1"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 1024
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

  config.vm.define "tower3" do |device|
    device.vm.hostname = "tower3"
    device.vm.box = "centos7"

    device.vm.synced_folder '.', '/vagrant', :disabled => true
    device.vm.provider :libvirt do |v|
      v.memory = 1024
    end
    device.vm.network "private_network",
      :ip => '10.1.3.10',
      :prefix => '24',
      :libvirt__forward_mode => 'veryisolated',
      :libvirt__dhcp_enabled => false,
      :libvirt__network_name => 'vault_net2'
    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
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
      :libvirt__tunnel_type => 'udp',
      :libvirt__tunnel_local_ip => '127.0.0.1',
      :libvirt__tunnel_local_port => '8010',
      :libvirt__tunnel_ip => '127.0.0.1',
      :libvirt__tunnel_port => '9010',
      :libvirt__iface_name => 'eth2',
      auto_config: false
    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
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
      :libvirt__network_name => 'vault_net2'

    device.vm.network "private_network",
      :libvirt__tunnel_type => 'udp',
      :libvirt__tunnel_local_ip => '127.0.0.1',
      :libvirt__tunnel_local_port => '8010',
      :libvirt__tunnel_ip => '127.0.0.1',
      :libvirt__tunnel_port => '9010',
      :libvirt__iface_name => 'eth2',
      auto_config: false
    device.vm.provision :ansible do |ansible|
      ansible.playbook = 'centos.yml'
    end


  end

end
