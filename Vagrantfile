# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # VirtualBox.
  config.vm.define "virtualbox" do |virtualbox|
    virtualbox.ssh.insert_key = false
    virtualbox.vm.synced_folder '.', '/vagrant', type: 'nfs'
    virtualbox.vm.hostname = "virtualbox-ubuntu1604"
    virtualbox.vm.box = "file://builds/virtualbox-ubuntu1604.box"
    virtualbox.vm.network :private_network, ip: "172.16.3.79"

    config.vm.provider :virtualbox do |v|
      v.gui = false
      v.memory = 1024
      v.cpus = 1
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
    end

    config.vm.provision "shell", inline: "echo Hello, World"
  end

  config.jsonconfig.load_json "variables.json"
  config.vm.define "vspherebox" do |vspherebox|
    vspherebox.vm.box = 'dummy'
    vspherebox.vm.box_url = 'https://vagrantcloud.com/ssx/boxes/vsphere-dummy/versions/0.0.1/providers/vsphere.box'
    vspherebox.ssh.password = "vagrant"

    vspherebox.vm.provider :vsphere do |vsphere|
        vsphere.host                  = config.jsonconfig.get "vsphere_server"
        vsphere.compute_resource_name = config.jsonconfig.get "vsphere_cluster"

        vsphere.template_name         = config.jsonconfig.get "vm_template"

        vsphere.user                  = config.jsonconfig.get "vsphere_user"
        vsphere.password              = config.jsonconfig.get "vsphere_password"
        vsphere.insecure              = true
    end

    config.vm.provision "shell", inline: "echo Hello, World"
  end

end
