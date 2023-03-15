# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
# 

$script = <<-SCRIPT
sudo yum install -y git vim
sudo useradd -s /bin/bash -d /opt/stack -m stack
sudo chmod +x /opt/stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
echo -e 'cd /opt/stack && git clone https://github.com/EuroLinux/openstack-devstack-el.git\ncd openstack-devstack-el && ./stack.sh' | tee /tmp/install.sh
sudo -u stack bash /tmp/install.sh
sleep 1m;
sudo sh -c "iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT && iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT && service iptables save"
sudo sh -c "iptables -I INPUT -p tcp -m tcp --dport 6080 -j ACCEPT && service iptables save"
sudo systemctl enable --now httpd
sudo systemctl enable --now memcached
reboot
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: $script
  config.vm.box = "eurolinux-vagrant/eurolinux-9"
  config.vm.provider "libvirt" do |vb|
    vb.memory = "8096"
    vb.cpus = 4
    vb.cpu_mode= 'host-passthrough'
    vb.nested= true
  end
end

