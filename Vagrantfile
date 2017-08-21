# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # Run on Centos 7
  config.vm.box = "centos/7"

  # Forward the elastic search port to the host machine
  config.vm.network "forwarded_port", guest: 9200, host: 9200

  config.vm.provision "shell", inline: <<-SHELL
    # Import the elastic search key and copy in it's yum repo information
    sudo rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch
    mv /vagrant/yum_files/elasticsearch.repo /etc/yum.repos.d/
    sudo yum install -y wget nano java-1.8.0-openjdk-devel.x86_64 elasticsearch
    # Start elasticsearch broadcasting to the world wide web
    echo "network.host: 0.0.0.0" | sudo tee -a /etc/elasticsearch/elasticsearch.yml
    sudo systemctl start elasticsearch
    sudo systemctl enable elasticsearch
    # Wait for the server to start (takes more than 10 seconds) and then create the index called 'joomla'
    sleep 15
    curl -XPUT 'http://localhost:9200/joomla/'
  SHELL
end
