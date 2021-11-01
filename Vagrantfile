# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_compose = <<-'SCRIPT'
  curl --silent -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /opt/bin/docker-compose
  sha256sum -c <<< "f3f10cf3dbb8107e9ba2ea5f23c1d2159ff7321d16f0a23051d68d8e2547b323  /opt/bin/docker-compose"
  chmod -v +x /opt/bin/docker-compose
  sha256sum /opt/bin/docker-compose
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    # Enable USB
    vb.customize ["modifyvm", :id, "--usbohci", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
    # Capture Conbee
    vb.customize ["usbfilter", "add", 0, "--name", "ConBee", "--target", :id, "--manufacturer", "FTDI"]
    # Capture Texas Instruments
    vb.customize ["usbfilter", "add", 1, "--name", "TICC2531", "--target", :id, "--manufacturer", "Texas Instruments"]
  end
  config.vm.network "public_network", ip: "192.168.87.38", bridge: "wlp0s20f3"
  config.vm.box = "flatcar-alpha"
  config.vm.provision "shell", inline: $install_compose
  config.vm.provision "shell", inline: 'ls -la /dev/tty???0'
  config.vm.provision "file", source: ".", destination: "~/home-assistant-demo"
end
