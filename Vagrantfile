# encoding: utf-8

Vagrant.configure('2') do |config|
  config.vm.box = 'opscode-centos-6.4'
  config.vm.box_url = 'https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_centos-6.4_provisionerless.box'

  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true

#  config.vm.define 'zookeeper' do |zookeeper|
#    zookeeper.vm.network :private_network, ip: '192.168.50.5'
#    zookeeper.vm.provider :virtualbox do |vb|
#      vb.customize ['modifyvm', :id, '--memory', '1024']
#    end

#    zookeeper.vm.provision :chef_solo do |chef|
#      chef.add_recipe 'java'
#      chef.add_recipe 'kafka::zookeeper'
#    end
#  end

  ['192.168.50.10', '192.168.50.11', '192.168.50.12'].each do |broker_node|
    config.vm.define broker_node do |broker|
      broker.vm.network :private_network, ip: broker_node
      broker.vm.provider :virtualbox do |vb|
        vb.customize ['modifyvm', :id, '--memory', '1024']
      end

      broker.vm.provision :chef_solo do |chef|
        chef.add_recipe 'java'
        chef.add_recipe 'kafka'
        chef.json = {
          'kafka' => {
            'zookeeper' => {
              'connect' => ['192.168.50.5:2181', '192.168.50.4:2181', '192.168.50.3:2181']
            }
          }
        }
      end
    end
  end
end
