IMAGE_NAME = "generic/ubuntu2010"
N = 3

Vagrant.configure("2") do |config|

  config.vm.synced_folder ".", "/vagrant"
  # config.vm.box_version = "4.3.6"
  config.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
  end

  config.vm.define "master" do |master|
    master.vm.box = IMAGE_NAME
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.56.10"
    master.vm.provider :virtualbox do |vbox|
      vbox.customize ["modifyvm", :id, "--natnet1", "10.3/16"]
    end
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/inventory/master_playbook.yaml"
    end
  end  

  (1..N).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.hostname = "node-#{i}"
      node.vm.network "private_network", ip: "192.168.56.#{10+i}"
      node.vm.provider :virtualbox do |vbox|
        vbox.customize ["modifyvm", :id, "--natnet1", "10.3/16"]
      end
      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/inventory/node_playbook.yaml"
      end
    end  
  end
end