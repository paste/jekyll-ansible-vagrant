# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
VAGRANT_IP = "192.168.33.11"

Vagrant.require_version ">= 1.8.2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/ubuntu-14.04"
  config.vm.network :private_network, ip: VAGRANT_IP
  config.vm.network :forwarded_port, guest: 4000, host: 80
  config.vm.synced_folder ".", "/home/vagrant/jav", type: "nfs"
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  # install Ansible within the VM, install Galaxy roles and run our playbook
  config.vm.provision "ansible_local" do |ansible|
    ansible.install = true
    ansible.provisioning_path = "/home/vagrant/jav/ansible"
    ansible.galaxy_role_file = "requirements.yml"
    ansible.playbook = "jav.yml"
  end

  # add localhost to Ansible inventory2
  config.vm.provision "shell", inline: "printf 'localhost\n' | sudo tee /etc/ansible/hosts > /dev/null"

  config.vm.provider "vmware_fusion" do |vf|
    vf.gui = true
    vf.vmx['displayname'] = "jav"
    vf.vmx["memsize"] = "1024"
    vf.vmx["numvcpus"] = "2"
  end

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.name = "jav"
    vb.memory = "1024"
    vb.cpus = 2
  end
end
