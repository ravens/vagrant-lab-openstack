## Virtualbox-based, all-in-one

You need virtualbox and vagrant for that one and some plugins to resize the initial disk image for convenience.

Preparation to your environment by editing the Vagrantfile :

* fix the public_interface variable that is used for bridging the VM with your real network (i.e. ens18f1)
* fix all the IP used in the Vagrantfile for what will be the public network in Openstack (i.e. 192.168.50.68), but also CIDR (192.168.50.0/24) and gateway info (192.168.50.253)

Then bringup the virtualbox VN
```
cd virtualbox-1node
vagrant up
```
