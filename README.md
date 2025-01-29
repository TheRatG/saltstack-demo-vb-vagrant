# SaltStack Demo VirtualBox & Vagrant

A SaltStack Demo using VirtualBox & Vagrant.

Inspired by: 
* https://github.com/UtahDave/salt-vagrant-demo.git
* https://github.com/vmware-archive/salty-vagrant.git


Instructions
============

Run the following commands in a terminal. Git, VirtualBox and Vagrant must
already be installed.

```bash
git clone https://github.com/TheRatG/saltstack-demo-vb-vagrant.git
cd saltstack-demo-vb-vagrant
vagrant plugin install vagrant-salt
vagrant up
```

This will download an Ubuntu  VirtualBox image and create three virtual
machines for you. One will be a Salt Master named `master` and two will be Salt
Minions named `minion1` and `minion2`.

You can then run the following commands to log into the Salt Master and begin
using Salt.

```bash
vagrant ssh master
sudo salt-key -a minion1
sudo salt-key -a minion2
sudo salt \* test.ping
```