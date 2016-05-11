Vagrant Plugins Required:
- vagrant-hostmanager

Python required => 2.7
Python Pkgs required: (pip)
shade

Deploys Training Guide Openstack Environment, using Vagrant, with variable compute nodes.

Each VM:
- 1 vCPUs
- 2GB RAM

-- Credentials --
Controller Node:
  MySQL/MariaDB:
    User: root
    Password: openstack
  ADMIN_TOKEN: openstack
  Keystone:
    User: keystone
    Password: openstack
  Glance:
    User: glance
    Password: openstack
  Demo:
    User: user
    Password: openstack

rabbit_host = controller
rabbit_userid = openstack
rabbit_password = openstack

-- rc files *on controller* --
/tmp/admin-openrc
/tmp/demo-openrc

Horizon Dashboard:
http://localhost:9080/dashboard
domain: default
user: admin
password: openstack
