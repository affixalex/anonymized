# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "hypoalex/alpine"
  config.vm.provider "virtualbox"
  config.vm.network :private_network, ip: "172.168.65.2"
  config.vm.synced_folder ".", "/web", type: "nfs"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apk update
    sudo apk upgrade
    sudo apk add alpine-sdk
    sudo apk add make
    sudo apk add gcc
    sudo apk add gperf
    sudo apk add vim
    sudo apk add openrc
    sudo apk add tor
    sudo apk add torsocks
    sudo apk add lynx
    sudo echo "SocksPort 9050" >> /etc/tor/torrc
    sudo echo "DNSPort 9053" >> /etc/tor/torrc
    sudo rc-update add tor
    
    sudo apk add dnsmasq
    sudo rm /etc/resolv.conf
    sudo echo "nameserver 127.0.0.1:9053" > /etc/resolv.conf
    sudo echo "nameserver 8.8.8.8" >> /etc/realresolv.conf
    sudo echo "nameserver 8.8.4.4" >> /etc/realresolv.conf
    sudo echo "port=9053" >> /etc/dnsmasq.conf
    sudo echo "resolv-file=/etc/realresolv.conf" >> /etc/dnsmasq.conf
    sudo echo "server=/onion/127.0.0.2" >> /etc/dnsmasq.conf
    sudo echo "server=127.0.0.1#9053" >> /etc/dnsmasq.conf
    sudo echo "listen-address=127.0.0.1" >> /etc/dnsmasq.conf
    sudo rc-update add dnsmasq

    echo "alias links='torsocks lynx -noreferer'" >> /etc/profile

    sudo openrc default # Start services.
  SHELL
end
