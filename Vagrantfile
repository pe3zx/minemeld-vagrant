# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1804"
  config.vm.hostname = "minemeld.vagrant.local"

  config.vm.provider "vmware_desktop" do |v, override|
    v.vmx["displayname"] = "Minemeld"
    v.memory = 4096
    v.cpus = 2
    v.gui = true
  end

  config.vm.provision "shell", inline: "sudo sed -i \"s/us.archive/th.archive/g\" /etc/apt/sources.list", privileged: true
  config.vm.provision "shell", inline: "apt update", privileged: true
  config.vm.provision "shell", inline: "apt upgrade -y", privileged: true
  config.vm.provision "shell", inline: "apt install -y gcc git python-minimal python2.7-dev libffi-dev libssl-dev make", privileged: true
  config.vm.provision "shell", inline: "apt install -y redis-server"
  config.vm.provision "shell", inline: "sudo sed -i \"s/bind 127.0.0.1 ::1/bind 127.0.0.1/g\" /etc/redis/redis.conf", privileged: true
  config.vm.provision "shell", inline: "wget https://bootstrap.pypa.io/get-pip.py", privileged: false
  config.vm.provision "shell", inline: "sudo -H python get-pip.py", privileged: true
  config.vm.provision "shell", inline: "sudo -H pip install ansible", privileged: true
  config.vm.provision "shell", inline: "git clone https://github.com/PaloAltoNetworks/minemeld-ansible.git", privileged: false
  config.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=0", privileged: true
  config.vm.provision "shell", inline: "cd minemeld-ansible && ansible-playbook -K -i 127.0.0.1, local.yml", privileged: true
  config.vm.provision "shell", inline: "sudo usermod -a -G minemeld $USER", privileged: true
  config.vm.provision "shell", inline: "apt install -y python-ujson", privileged: true
  config.vm.provision "shell", inline: "cd /opt/minemeld/engine/current/lib/python2.7/site-packages/ && sudo mv ujson.so ujson.so.old && sudo ln -s /usr/lib/python2.7/dist-packages/ujson.x86_64-linux-gnu.so ujson.so && chmod 777 ujson.so", privileged: true
  config.vm.provision "shell", inline: "sudo -u minemeld /opt/minemeld/engine/current/bin/supervisorctl -c /opt/minemeld/supervisor/config/supervisord.conf restart all", privileged: true
end 