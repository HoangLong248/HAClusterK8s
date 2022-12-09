
Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
    systemctl reload sshd
    echo -e "admin\nadmin" | passwd root > /dev/null 2>&1
  SHELL

  (1..3).each do |i|
    config.vm.define "etcd#{i}" do | node |
      node.vm.box = "bento/ubuntu-20.04"
      node.vm.hostname = "etcd#{i}"
      node.vm.network "private_network", ip: "172.16.16.20#{i}"
      node.vm.provider "virtualbox" do | vb |
        vb.name = "etcd#{i}"
        vb.memory = 1024
        vb.cpus = 1
      end
    end
  end
end
