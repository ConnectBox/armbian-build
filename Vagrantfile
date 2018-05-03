# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.5"

$provisioning_script = <<SCRIPT
# use remote git version instead of sharing a copy from host to preserve proper file permissions
# and prevent permission related issues for the temp directory
git clone https://github.com/ConnectBox/armbian-build.git /home/vagrant/armbian
mkdir -p /vagrant/output /vagrant/userpatches
ln -sf /vagrant/output /home/vagrant/armbian/output
ln -sf /vagrant/userpatches /home/vagrant/armbian/userpatches
SCRIPT

Vagrant.configure(2) do |config|

    # What box should we base this build on?

    #######################################################################
    # THIS REQUIRES YOU TO INSTALL A PLUGIN. RUN THE COMMAND BELOW...
    #
    #   $ vagrant plugin install vagrant-disksize
    #
    # Default images are not big enough to build Armbian.
    config.disksize.size = "40GB"

    # provisioning: install dependencies, download the repository copy
    config.vm.provision "shell", inline: $provisioning_script

    # forward terminal type for better compatibility with Dialog - disabled on Ubuntu by default
    config.ssh.forward_env = ["TERM"]

    config.vm.provider "vmware_fusion" do |v|
      config.vm.box = "bento/ubuntu-16.04"
      v.vmx["memsize"] = 8192
      v.vmx["numvcpus"] = 4
    end

    config.vm.provider "virtualbox" do |vb|
        vb.name = "Armbian Builder"
	config.vm.box = "ubuntu/xenial64"
	config.vm.box_version = ">= 20180126.0.0"

        # uncomment this to enable the VirtualBox GUI
        #vb.gui = true

        # Tweak these to fit your needs.
        #vb.memory = "8192"
        #vb.cpus = "4"
    end

end
