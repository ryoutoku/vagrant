# -*- mode: ruby -*-
# vi: set ft=ruby :

proxy = ""
porxy_port = ""

Vagrant.configure("2") do |config|

  # 使用するboxの設定
  # Ubuntu server 14.04
  config.vm.box = "ubuntu/trusty64"

  # ポート
  config.vm.network "forwarded_port", guest: 8065, host: 8065

  # ホストオンリーアダプタをipアドレス固定で追加
  config.vm.network "private_network", ip: "192.168.56.101"

  # virtualboxの実行設定
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "2048"
  end

  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = "http://#{proxy_ip}:#{proxy_port}"
    config.proxy.https    = "http://#{proxy_ip}:#{proxy_port}"
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get remove docker docker-engine docker.io

    apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
    apt-get install apt-transport-https ca-certificates curl software-properties-common aptitude
    curl -O https://download.docker.com/linux/ubuntu/dists/trusty/pool/stable/amd64/docker-ce_18.03.1~ce-0~ubuntu_amd64.deb
    dpkg -i docker-ce_18.03.1~ce-0~ubuntu_amd64.deb
	
	# 依存関係が壊れるらしいので必要
	apt-get -f -y install
    docker run --restart=always --name mattermost-preview -d --publish 8065:8065 mattermost/mattermost-preview

  SHELL
end