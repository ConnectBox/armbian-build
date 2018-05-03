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
    config.vm.box = "ubuntu/bionic64"

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
      config.vm.box = "bento/ubuntu-18.04"
      config.vm.box_version = ">= 201808.24.0"
      v.vmx["memsize"] = 8192
      v.vmx["numvcpus"] = 4
    end

end
