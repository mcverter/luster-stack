***VAGRANT***
=============

Machine virtualization tool

RESOURCES
=========
[Vagrant Documentation](https://www.vagrantup.com/docs/index.html)

Vagrant Usage
=============
* up and running
```
$ vagrant init hashicorp/precise64
$ vagrant up
```

Project Setup
=============
* Vagrantfile:  rootdir, machine-type
* Installing a box (base image)
```
$ vagrant box add hashicorp/precise64
(username,boxname)
```
* Vagrantfile
```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"

end
```
* vagrant up
* vagrant ssh
* > logout
* vagrant destroy (terminate resources)

Sync Folders
============
* typically shares project directory where Vagrant file is to /vagrant directory in guest
* /home/vagrant is not /vagrant

Provisioning
============
* Automated provisioning (daemon services, etc)
* bootstrap.sh
```
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
```
* Vagrantfile
```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```


