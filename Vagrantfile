Vagrant.configure("2") do |config|

   config.vm.provision "file", source: "keys/myKey.pub", destination: "/home/vagrant/.ssh/"

   config.vm.define "controlnode" do |controlnode|
     controlnode.vm.box = "ubuntu/focal64"
     controlnode.vm.hostname = "controlnode"
     controlnode.vm.network "private_network", ip: "192.168.56.4"
     controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
     controlnode.vm.provision "file", source: "keys/myKey", destination: "/home/vagrant/.ssh/"
     controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      apt install -y ansible
      sudo -u vagrant sh -c "ansible-galaxy collection install community.postgresql"
      sudo -u vagrant sh -c "ansible-galaxy role install geerlingguy.nginx"
      chmod 0600 /home/vagrant/.ssh/myKey
      chmod 0644 /home/vagrant/.ssh/myKey.pub
    SHELL
   end

   config.vm.define "server_centos" do |server_centos|
     server_centos.vm.box = "bento/centos-7.9"
     server_centos.vm.hostname = "centos"
     server_centos.vbguest.auto_update = false
     server_centos.vm.network "private_network", ip: "192.168.57.3"
     server_centos.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/myKey.pub
      cat /home/vagrant/.ssh/myKey.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
   end
 end
