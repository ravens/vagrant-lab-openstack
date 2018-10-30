# vagrant-lab-openstack
Some Vagrantfiles to build various version of Openstack. We focus on the Rocky release, using the containers based version (Kolla) to speedup deployment.

## virtualbox-based, all-in-one lab

You need virtualbox and vagrant for that one.
Preparation to your environment by editing the Vagrantfile :

* fix the public_interface variable that is used for bridging the VM with your real network (i.e. ens18f1)
* fix all the IP used in the Vagrantfile for what will be the public network in Openstack (i.e. 192.168.50.68), but also CIDR (192.168.50.0/24) and gateway info (192.168.50.253)

Bring up the VM :
```
vagrant up
```

Should take less than 10min on a fast connection. 

Access the openstack CLI client and run a demo VMs : 
```
vagrant ssh
source /etc/kolla/admin-openrc.sh
openstack server create --image cirros --flavor m1.tiny --key-name mykey --network demo-net demo1
```
