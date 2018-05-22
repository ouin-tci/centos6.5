# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.hostname = "vagrant-local-mentough"
  config.vm.box = "centos6.5-x86_64-minimal_20131205"
  config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box"

  config.vm.synced_folder ".", "/vagrant", nfs: true
  config.vm.network :private_network, ip: "192.168.33.10"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", 2048]
    # XXX improve network speed. http://qiita.com/s-kiriki/items/357dc585ee562789ac7b
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    vb.name = "local-mentough"
  end

  # MTOP2(3000), mentough(3001), mentough_en(3002), feap2(3003)
  # postgres(5432 -> 4432), mailcathcer(1080 -> 1080)
  config.vm.network :forwarded_port, guest: 3000, host: 3000
  config.vm.network :forwarded_port, guest: 3001, host: 3001
  config.vm.network :forwarded_port, guest: 3002, host: 3002
  config.vm.network :forwarded_port, guest: 3003, host: 3003
  config.vm.network :forwarded_port, guest: 5432, host: 4432
  config.vm.network :forwarded_port, guest: 1080, host: 1080
end
