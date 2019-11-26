ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
IMAGE_NAME = "generic/ubuntu1804"
n = 2
master_node = []
slave_nodes = []
host_vars = {}

Vagrant.configure("2") do |config|

    config.vm.provider "libvirt" do |v|
        v.memory = 4096
        v.cpus = 2
        v.storage :file, :size => '50G'
    end

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        master_node << "k8s-master"
        host_vars["k8s-master"] = { "ansible_host" => "192.168.50.10", "ansible_port" => 22, "node_ip" => "192.168.50.10" }
    end

    (1..n).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            slave_nodes << "node-#{i}"
            host_vars["node-#{i}"] = { "ansible_host" => "192.168.50.#{i + 10}", "ansible_port" => 22, "node_ip" => "192.168.50.#{i + 10}" }
            if i == n
                node.vm.provision "ansible" do |ansible|
                    ansible.host_vars = host_vars
                    ansible.groups = {
                        "master_node" => master_node,
                        "slave_nodes" => slave_nodes,
                    }
                    ansible.playbook = "kubernetes-setup/master-tasks.yml"
                    ansible.limit = "master_node"
                end
                node.vm.provision "ansible" do |ansible|
                    ansible.playbook = "kubernetes-setup/node-tasks.yml"
                    ansible.host_vars = host_vars
                    ansible.groups = {
                        "master_node" => master_node,
                        "slave_nodes" => slave_nodes,
                    }
                    ansible.limit = "slave_nodes"
                end
            end
        end
        config.vm.provision 'shell', inline: "install -m 0700 -d /root/.ssh/; echo #{ssh_pub_key} >> /root/.ssh/authorized_keys; chmod 0600 /root/.ssh/authorized_keys"
        config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: false
        config.vm.provision 'shell', inline: "apt update"
        config.vm.provision 'shell', inline: "apt dist-upgrade -y"
    end
end
