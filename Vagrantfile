# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.box_check_update = true
    config.vm.provider "virtualbox" do |vb|
      vb.customize ['modifyvm', :id, '--nested-hw-virt', 'on']
    end
    config.vm.provision "shell", inline: "sed -i '/#Password.*/s/^#//' /etc/ssh/sshd_config"
    config.vm.provision "shell", inline: "systemctl restart sshd"
    config.vm.provision "shell", inline: "mkdir /mnt/glusterfs"
  
    $instance=2
    (1..$instance).each do |i|
           config.vm.define "glusterfs#{i}" do |node|
             node.vm.hostname =  "glusterfs#{i}"
             node.vm.network "private_network", ip: "10.10.10.#{i+200}"
             node.vm.disk :disk, name: "gluster", size: "2GB"
             node.vm.provider "virtualbox" do |v|
              unless File.exist?("diskgfs#{i}.vdi")
                v.customize ['createhd', '--filename', "diskgfs#{i}", '--variant', 'Fixed', '--size', 1 * 1024]
              end
              v.customize ['storageattach', :id,  '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', "diskgfs#{i}.vdi"]
              v.name = "glusterfs#{i}"
              v.memory = 1024
              v.cpus = 1
             end
           end
    end
  end
  