# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["usbfilter", "add", "0", 
                  "--target", :id, 
                  "--name", "FTDI-Connection-to-ESP8266",
                  "--productid", "6001",
                  "--vendorid", "0403",
                 ]
  end
  config.vm.provision "devenv", type: "shell", inline: <<-SHELL
     id
     apt-get update -y

     apt-get install -y linux-image-extra-`uname -r`
     adduser vagrant dialout

     apt-get install -y linux-image-extra-virtual # for usbserial
     apt-get install -y git # for git version
     apt-get install -y python python-setuptools python-pip # python2
     pip2 install -U platformio
     VAGRANT=vagrant
     if ! grep "$VAGRANT" /etc/passwd; then
       VAGRANT=ubuntu
     fi

     sudo -u $VAGRANT -H bash -c "echo 'cd /vagrant' >>~$VAGRANT/.bash_login"
     sudo -u $VAGRANT -H bash -c "echo 'echo Try: platformio run -e esp01 -t upload' >>~$VAGRANT/.bash_login"

     sudo -u $VAGRANT -H bash -c "cd /vagrant; platformio run"
# platformio run -e esp01 -t upload
   SHELL
end


# cd /vagrant
# platformio run -e esp01 -t upload
