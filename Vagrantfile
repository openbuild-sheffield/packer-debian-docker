Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.synced_folder '.', '/vagrant', type: 'nfs'
  config.vm.define "virtualbox" do |virtualbox|
    virtualbox.vm.hostname = "virtualbox-debian9"
    virtualbox.vm.box = "file://builds/virtualbox-debian9.box"
    virtualbox.vm.network :private_network, ip: "172.16.3.2"
    config.vm.provider :virtualbox do |v|
      v.gui = false
      v.memory = 1024
      v.cpus = 1
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
    end
    config.vm.provision "shell", inline: "echo Welcome to debian9"
  end
end