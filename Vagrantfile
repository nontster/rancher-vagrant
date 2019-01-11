Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu1804"

  config.vm.network "private_network", ip: "172.17.8.100"

  config.vm.hostname = "rancher" 

  config.vm.provider "virtualbox" do |vb|
    vb.name = "rancher"
    vb.memory = "1024"
    vb.cpus = "2"
    vb.gui = false     
  end

  config.vm.provision "shell", inline: <<-SHELL
    rm /etc/resolv.conf
    ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
  SHELL

  config.vm.provision "docker" do |d|
    d.pull_images "rancher/server"
    d.run "rancher/rancher:stable",
      daemonize: true,
      restart: "unless-stopped",
      args: "--name rancher -p 80:80 -p 443:443"
  end
end
