$script = <<-SCRIPT                                                            
sudo sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/" /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo useradd kami
sudo echo "welcome1" |passwd --stdin kami
sudo mkdir /home/kami/.ssh
sudo cp /tmp/authorized_keys /home/kami/.ssh/authorized_keys
sudo chown kami:kami /home/kami/.ssh/authorized_keys
sudo su -
echo -e "kami\tALL=(ALL)\tNOPASSWD: ALL"> /etc/sudoers.d/kami
exit
SCRIPT

Vagrant.configure(2) do |config|
        #Basic Configuration
        config.vm.box = "centos/7"
	config.vm.provision "file", source: "~/vagrant-project/id_rsa.pub", destination: "/tmp/authorized_keys"
	config.vm.provision "shell", inline: $script

        (1..2).each do |i|
                vmName = "ansible-client#{i}"
                vmIP = "192.168.1.2#{i}"
                config.vm.define vmName do |server|
                        server.vm.hostname = vmName
                        server.vm.network "public_network", ip: vmIP
                end
        end
end
