VAGRANTFILE_API_VERSION = "2"

required_plugins = %w(vagrant-cachier)
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use Ubuntu 14.04 Trusty Tahr 32-bit as our operating system
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider :virtualbox do |vb|
    # Optimizing memory and CPU
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    # Optimizing network
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.name = 'demo'
  end

  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :box
    config.cache.enable :apt
    config.cache.enable :apt_lists
    config.cache.enable :npm
    config.cache.enable :gem
    config.cache.synced_folder_opts = {
      type: :nfs,
      # The nolock option can be useful for an NFSv3 client that wants to avoid the
      # NLM sideband protocol. Without this option, apt-get might hang if it tries
      # to lock files needed for /var/cache/* operations. All of this can be avoided
      # by using NFSv4 everywhere. Please note that the tcp option is not the default.
      mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
    }
  end
  
  config.ssh.insert_key = false
    config.vm.define :demo do |demo|
  end
  
  config.vm.network :private_network, ip: "192.168.33.10"
  # Optimize performance: Use NFS for folder sync
  config.vm.synced_folder ".", "/ems", type: "nfs"

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.inventory_path = "provisioning/inventory"
    ansible.sudo = true
  end
    
  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true
  config.vm.network "forwarded_port", guest: 3000, host: 3000, auto_correct: true
end
