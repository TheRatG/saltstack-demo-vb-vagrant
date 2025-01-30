# SaltStack Demo VirtualBox & Vagrant

A SaltStack Demo using VirtualBox & Vagrant.

Inspired by: 
* https://github.com/UtahDave/salt-vagrant-demo.git
* https://github.com/vmware-archive/salty-vagrant.git


## Instructions

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
sudo salt-key -a minion1-project1
sudo salt-key -a minion2-project2
sudo salt \* test.ping
```

## Glossary

https://docs.saltproject.io/en/latest/glossary.html

### Windows

`vagrant ssh master` - error https://www.vagrantup.com/docs/other/environmental-variables.html#vagrant_prefer_system_bin
```
vagrant@127.0.0.1: Permission denied (publickey).
```

```bash
$Env:VAGRANT_PREFER_SYSTEM_BIN += 0
```

### Minions

Generate keys example:
```
openssl genrsa -out minion3.pem 2048
openssl rsa -in minion3.pem -pubout -out minion3.pub
```
