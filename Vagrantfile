# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
    v.cpus = 2
    v.memory = 1024
  end

  config.vm.network "forwarded_port", guest: 4200, host: 4200
  config.vm.network "forwarded_port", guest: 35729, host: 35729

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo apt-get update
    sudo apt-get autoremove -y
    sudo apt-get upgrade -y
    sudo apt-get install -y \
      build-essential \
      openssl \
      libssl-dev \
      pkg-config \
      git
    curl https://raw.githubusercontent.com/creationix/nvm/v0.24.1/install.sh | bash
    source ~/.bashrc
    source ~/.nvm/nvm.sh
    nvm install 0.12
    echo "nvm use 0.12" >> ~/.bashrc
    npm i -g npm@latest
    npm i -g npm_lazy
    npm_lazy --init \
      | perl -pi -e 's/cacheAge: 0/cacheAge: -1/' \
      | perl -pi -e 's/logToConsole: true/logToConsole: false/' \
      > ~/npm_lazy.config.js
    npm_lazy --config ~/npm_lazy.config.js &
    npm config set registry http://localhost:8080/
    npm i -g ember-cli
  SHELL
end
