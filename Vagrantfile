# _*_ mode: ruby _*_

Vagrant.configure("2") do |config|

    (1..1).each do |i|
        config.vm.define "dev#{i}" do |subconfig|
            subconfig.vm.box = "ubuntu/focal64"
            subconfig.vm.hostname = "dev#{i}"
            subconfig.vm.network "private_network", ip: "192.168.60.10#{i}"
            subconfig.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible/dev.yml"
            end
        end
    end

    config.vm.define "ci-server" do |subconfig|
        subconfig.vm.box = "centos/7"
        subconfig.vm.hostname = "jenkins"
        subconfig.vm.network "private_network", ip: "192.168.60.254"
        subconfig.vm.provider "virtualbox" do |vb|
            vb.memory = "6192"
        end
        subconfig.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/ci-server.yml"
        end
        subconfig.vm.provision "file", source: "./id_rsa", destination: "/home/vagrant/.ssh/"
    end

    config.vm.define "prod-server" do |subconfig|
        subconfig.vm.box = "centos/7"
        subconfig.vm.hostname = "prod"
        subconfig.vm.network "private_network", ip: "192.168.60.250"
        subconfig.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/prod-server.yml"
        end
    end

    config.vm.define "staging-server" do |subconfig|
        subconfig.vm.box = "centos/7"
        subconfig.vm.hostname = "staging"
        subconfig.vm.network "private_network", ip: "192.168.60.200"
        subconfig.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/staging-server.yml"
        end
        subconfig.vm.provision "shell", inline: $script_inject_pk
    end
end

$script_inject_pk =<<-'EOF'
cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
EOF