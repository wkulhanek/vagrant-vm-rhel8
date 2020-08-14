# -*- mode: ruby -*-
# vi: set ft=ruby :

# RHEL 8 Vagrant box:
box = "generic/rhel8"

# set to taste
hostname = "rhel8vm"
ip = "192.168.10.10"

# optional - set to taste
local_folder = "~/Development"
remote_mount = "/home/vagrant/development"

# set with one or more keys
ssh_keys = ["~/.vagrant.d/insecure_private_key", "~/.ssh/vagrant.pem" ]

memory = 4096

Vagrant.configure("2") do |config|
  config.vm.box = box

  # turn off auto-updating of vbox additions
  # issue for RHEL as yum can't run until after subscription manager has
  # Use "vagrant vbguest" from the command line after the VM has been started
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  # setup networking and hostname - works best with vagrant hostsupdater plugin
  config.vm.hostname = "%s.v" % hostname # domain postfix optional good for ssh "*.v" etc
  config.vm.network "private_network", ip: ip
#  config.vm.network "forwarded_port", guest: 80, host: 80
#  config.vm.network "forwarded_port", guest: 443, host: 443
#  config.vm.network "forwarded_port", guest: 8443, host: 8443

  # setup ssh keys, use own instead of allowing vagrant to generate one
  config.ssh.insert_key = false
  config.ssh.private_key_path = ssh_keys

  # setup any shared folders, set in global vars
  config.vm.synced_folder local_folder, remote_mount

  # Disable default /vagrant share
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # setup virtualbox settings
  config.vm.provider :virtualbox do |vb|
    vb.name = hostname
    vb.memory = memory
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "configure_box.yml"
    ansible.inventory_path = "./inventory"
    ansible.extra_vars = {
      ansible_python_interpreter: "/usr/libexec/platform-python"
    }
  end
end
