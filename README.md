Ansible odds and sods
=====================

Various small Vagrant VMs and assorted Ansible roles.
Not intended to be a full deployment, more a testbed for
various bits of software I'm currently trialling.

  * gitlab CE [gitlab.md](gitlab.md)
  * nexus (incomplete)

## base OS

CentOS 6.x x64 (from https://github.com/box-cutter/centos-vm ) -
see Vagrantfile for precise version.

## requirements

* the centos base image above
* Ansible (2.x) on your host machine
* Virtualbox on host machine (box has 4.3.28 extensions)
* Vagrant 1.8.x or better (for linked_clones)
* 2 Vagrant plugins (see below)

## assumptions

* Hosts permit passwordless ssh+sudo for the relevant ansible_ssh_user.
* hosts connect to each others inventory hostname
* DNS works (see previous point)

## Vagrant setup

an inventory for Vagrant is in *vagrant/hosts*, the hostnames
in there need to match your Vagrantfile.


### name resolution

we'll use a Vagrant plugin to ensure all nodes (and our host) can resolve each others names.

The [hostmanager plugin](https://github.com/smdahlen/vagrant-hostmanager) will auto-manage that.

    vagrant plugin install vagrant-hostmanager

### do the thing

    vagrant up

_NB: hostmanager will (try to) edit your local /etc/hosts_

### wiping 

If you want to blow everything away, this should do the trick:

    vagrant destroy -f
    for i in nexus1 gitlab1
      do
        ssh-keygen -R $i
      done

## time for Ansible

run the main play with:

    ansible-playbook -i vagrant/ site.yml

If you add/destroy vagrant VMs, the 'vagrant up' should
auto-manage your local /etc/hosts along with existing VMs. If you
need to ensure it's up to date, just run

    vagrant hostmanager

## vars

* role-specific vars live in $rolename/defaults/main.yml
* 'environment' specific vars _(e.g. ansible_ssh_user)_ live in $inventory/group_vars/all
  and override role defaults

### BUGS

Oh, I expect so. Log an issue / PR if you notice any.
