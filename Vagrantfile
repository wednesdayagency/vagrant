Vagrant.configure("2") do |config|
  config.vm.box = "wednesday"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.network :private_network, ip: "192.168.56.101"
    config.vm.network :forwarded_port, guest: 80, host: 8080
    config.vm.network :forwarded_port, guest: 3306, host: 3309
    config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--memory", 1024]
    v.customize ["modifyvm", :id, "--name", "wednesday-ubuntu-apache-mysql"]
  end

  
  #config.vm.synced_folder "./", "/var/www", id: "vagrant-root" 
  config.vm.synced_folder "~/src/sites", "/var/www", id: "vagrant-root" 
  config.vm.provision :shell, :inline =>
    "if [[ ! -f /apt-get-run ]]; then sudo apt-get update && sudo touch /apt-get-run; fi"

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.options = ['--verbose']
  end
end
