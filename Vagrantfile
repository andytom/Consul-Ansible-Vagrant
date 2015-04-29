# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  # Stop the annoying 'stdin: is not a tty' messages
  # See https://github.com/mitchellh/vagrant/issues/1673
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.ssh.insert_key = false

  config.vm.define 'client1' do |machine|
    machine.vm.hostname = 'client1'
    machine.vm.network "private_network", ip: "192.168.20.13"
  end

  config.vm.define 'node3' do |machine|
    machine.vm.hostname = 'node3'
    machine.vm.network "private_network", ip: "192.168.20.12"
  end
  
  config.vm.define 'node2' do |machine|
    machine.vm.hostname = 'node2'
    machine.vm.network "private_network", ip: "192.168.20.11"
  end

  config.vm.define 'node1' do |machine|
    machine.vm.hostname = 'node1'
    machine.vm.network "private_network", ip: "192.168.20.10"
    # Do all the provisioning in one go
    # See https://docs.vagrantup.com/v2/provisioning/ansible.html
    machine.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/site.yml"
      ansible.groups = {
        'consul_client' => ['client1'],
        'consul_server' => ['node1', 'node2', 'node3'],
        'consul:children' => ['consul_server', 'consul_client']
      }
      ansible.limit = 'all'
    end
  end
end
