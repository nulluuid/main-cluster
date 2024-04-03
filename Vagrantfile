Vagrant.configure("2") do |config|

    # Provision Scripts
    system_upgrade = <<-SHELL
      sudo apt-get update -y
      sudo apt-get upgrade -y
    SHELL

    # Installing control server
    
    config.vm.define "control" do |control|
        control.vm.network "private_network", ip: "192.168.0.50"
        control.vm.provision "shell", inline: system_upgrade
        control.vm.provider "virtualbox" do |vb|
            vb.memory = "512"
            vb.cpus = 1
        end
    end

    # Define the common settings for all nodes
    config.vm.box = "debian/bookworm64"
    config.vm.box_check_update = false
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.cpus = 1
    end

    # Define the number of nodes
    num_nodes = 3

    # Loop to create and configure each node
    (0..num_nodes-1).each do |i|
      config.vm.define "node#{i}" do |node|
        node.vm.network "private_network", ip: "192.168.0.#{i+10}"
        node.vm.provision "shell", inline: system_upgrade
      end
    end
  
  end
  