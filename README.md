# vagrant-lab-openstack
Some Vagrantfiles to build various version of Openstack. We focus on the Rocky release, using the containers based version (Kolla) to speedup deployment.

## virtualbox-based, all-in-one lab

This lab uses a single VM to host a complete Openstack install. 

## virtualbox-based, 3 nodes 

This lab uses a 3 VMs to host an Openstack with 2 controllers and one compute node.

## libvirt-based, 3 nodes 

This lab uses a 3 VMs using libvirt as hypervisor to host an Openstack with 2 controllers and one compute node. 
Warning : due to to the use of macvlan based interfaces; the neutron external interface does not work yet. (FIXME)