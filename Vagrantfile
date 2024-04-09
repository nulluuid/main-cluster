
require 'yaml'
require 'securerandom'
require 'fileutils'

def create_meta_data(hostname)
    data = {
        'instance-id' => "id-#{SecureRandom.hex(8)}",    
        'local-hostname' => hostname,
    }

    File.open('tmp/meta-data', 'w') do |file|
        file.write(YAML.dump(data))
    end
end

def create_user_data(type)
    FileUtils.copy("#{type}.yaml", "tmp/user-data")
end

def create_iso(hostname, type)
    puts "Creating iso for #{hostname}"
    create_meta_data(hostname)
    create_user_data(type)
    system('mkisofs -joliet -rock -volid "cidata" -output tmp/nocloud.iso tmp/meta-data tmp/user-data')
end

Vagrant.configure("2") do |config|

    # Define the number of nodes
    num_nodes = 3

    config.vm.define "control" do |control|
        control.vm.box = "debian12"
        control.trigger.before :up do |trigger|
            trigger.ruby do |env,machine|
                create_iso("control", "control")
            end
        end
        control.vm.hostname = "control"
        control.vm.network "private_network", ip: "192.168.0.50"
        control.vm.provider :virtualbox do |vb|
            vb.memory = "1024"
            vb.cpus = 1
            vb.customize [
                "storageattach", :id,
                "--storagectl", "SATA",
                "--port", "1",
                "--type", "dvddrive",
                "--medium", "tmp/nocloud.iso"
            ]
        end
    end

    (0..num_nodes-1).each do |i|
        config.vm.define "node#{i}" do |node|
            node.trigger.before :up do |trigger|
                trigger.ruby do |env,machine|
                    create_iso("node#{i}", "node")
                end
            end
            node.vm.box = "debian12"
            node.vm.hostname = "node#{i}"
            node.vm.network "private_network", ip: "192.168.0.#{i+10}"
            node.vm.provider :virtualbox do |vb|
                vb.memory = "1024"
                vb.cpus = 1
                vb.customize [
                    "storageattach", :id,
                    "--storagectl", "SATA",
                    "--port", "1",
                    "--type", "dvddrive",
                    "--medium", "tmp/nocloud.iso"
                ]
            end
        end
    end
end
