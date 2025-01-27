# Test your role with vagrant

Test your role with vagrant

## Prerequisites

  * Virtualbox is installed
  * Vagrant is installed
  * vagrant up will install vagrant-hostmanager module

## Test process

```
cd vagrant
vagrant up
```

The vagrant role will be applied automatically during the vagrant up process.

### Test with molecule from inside vagrant

```
vagrant ssh
cd /etc/ansible/roles/ansible-role-odoo
molecule test --all
```

### Test the installation

You can test the several domains:

  * http://odoo.lokal:8069
  * http://demo.odoo.lokal:8069
  * http://mail.odoo.lokal

Default username and password are:

```
Email: admin
Password: admin
```

### Manually initialize the odoo database

```
sudo -u odoo /usr/bin/python3 /usr/bin/odoo --config /etc/odoo/odoo.conf --database odoo --init base --load-language de_DE --without-demo all --stop-after-init
```
