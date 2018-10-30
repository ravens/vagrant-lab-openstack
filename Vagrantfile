# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  $physical_interface = "ens18f1"
  $public_ip = "192.168.50.68" 

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "public_network", bridge: $physical_interface, ip: $public_ip 

  config.vm.network "public_network", bridge: $physical_interface, auto_config: false

  config.vm.provider "virtualbox" do |v|
      v.name = "kolla-allinone"
      v.memory = 8192 
      v.cpus = 2
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"] 
      v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
  end

  config.vm.provision "shell", inline: <<-SHELL
apt-get update
apt-get -qy install python-pip
apt-get -qy install python-dev libffi-dev gcc libssl-dev python-selinux python-setuptools
pip install ansible 
pip install kolla-ansible
mkdir /etc/ansible
cat << 'EOF' | sudo tee /etc/ansible/ansible.cfg
[defaults]
host_key_checking=False
pipelining=True
forks=100
EOF
cp -r /usr/local/share/kolla-ansible/etc_examples/kolla /etc/ 
cp /usr/local/share/kolla-ansible/ansible/inventory/* /vagrant
sed -i s/'#openstack_release: ""'/'openstack_release: "rocky"'/g /etc/kolla/globals.yml
sed -i s/'kolla_internal_vip_address: "10.10.10.254"'/'kolla_internal_vip_address: "192.168.50.68"'/g /etc/kolla/globals.yml
sed -i s/'#network_interface: "eth0"'/'network_interface: "enp0s8"'/g /etc/kolla/globals.yml
sed -i s/'#enable_haproxy: "yes"'/'enable_haproxy: "no"'/g /etc/kolla/globals.yml
sed -i s/'#neutron_external_interface: "eth1"'/'neutron_external_interface: "enp0s9"'/g /etc/kolla/globals.yml
sed -i s/'#nova_compute_virt_type: "kvm"'/'nova_compute_virt_type: "qemu"'/g /etc/kolla/globals.yml
kolla-genpwd 
cp /etc/kolla/passwords.yml /vagrant/
ifconfig enp0s9 up
kolla-ansible -i /vagrant/all-in-one bootstrap-servers
kolla-ansible -i /vagrant/all-in-one prechecks
kolla-ansible -i /vagrant/all-in-one deploy
kolla-ansible -i /vagrant/all-in-one post-deploy
SHELL

# fixing vagrant local configuration; adding vagrant user to docker so that we can control containers
config.vm.provision "shell", inline: <<-SHELL
adduser vagrant docker
echo 'LC_ALL="en_US.UTF-8"'  >>  /etc/default/locale
echo 'LC_CTYPE="en_US.UTF-8"'  >>  /etc/default/locale
SHELL

# install client part of openstack, and run demo examples
config.vm.provision "shell", inline: <<-SHELL
pip install python-openstackclient
source /etc/kolla/admin-openrc.sh
sed -i s,"EXT_NET_CIDR='10.0.2.0/24'","EXT_NET_CIDR='192.168.50.0/24'",g  /usr/local/share/kolla-ansible/init-runonce
sed -i s/"EXT_NET_RANGE='start=10.0.2.150,end=10.0.2.199'"/"EXT_NET_RANGE='start=192.168.50.150,end=192.168.50.170'"/g /usr/local/share/kolla-ansible/init-runonce
sed -i s/"EXT_NET_GATEWAY='10.0.2.1'"/"EXT_NET_GATEWAY='192.168.50.253'"/g /usr/local/share/kolla-ansible/init-runonce
/usr/local/share/kolla-ansible/init-runonce
SHELL

end
