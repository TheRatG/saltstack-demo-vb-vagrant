# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  os = "ubuntu/jammy64"
  os_version = "20241002.0.0"

  net_ip = "192.168.56"

  config.vm.define :master, primary: true do |master_config|
    master_config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
        vb.name = "master"
    end

    master_config.vm.box = "#{os}"
    master_config.vm.box_version = "#{os_version}"
    master_config.vm.host_name = 'saltmaster.local'
    master_config.vm.network "private_network", ip: "#{net_ip}.10"
    master_config.vm.synced_folder "saltstack/salt/", "/srv/salt"
    master_config.vm.synced_folder "saltstack/pillar/", "/srv/pillar"

    master_config.ssh.insert_key = false

    master_config.vm.provision :salt do |salt|
      salt.master_config = "saltstack/etc/master"
      salt.master_key = "saltstack/keys/master_minion.pem"
      salt.master_pub = "saltstack/keys/master_minion.pub"
      salt.minion_key = "saltstack/keys/master_minion.pem"
      salt.minion_pub = "saltstack/keys/master_minion.pub"
      salt.seed_master = {
                          "minion1" => "saltstack/keys/minion1.pub",
                          "minion2" => "saltstack/keys/minion2.pub",
                          "minion3" => "saltstack/keys/minion3.pub"
                         }

      salt.install_type = "stable"
      salt.install_args = "3006.9"
      salt.install_master = true
      salt.no_minion = true
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_script = "scripts/bootstrap-salt.sh"
      salt.bootstrap_options = "-P -c /tmp -x python3"
    end
  end


  [
    ["minion1", "project1",   "#{net_ip}.11",    "512",    os ],
    ["minion2", "project1",   "#{net_ip}.12",    "512",    os ],
    ["minion3", "project2",   "#{net_ip}.13",    "512",    os ],
  ].each do |minion,project,ip,mem,os|
    config.vm.define "#{minion}-#{project}" do |minion_config|
      minion_config.vm.provider "virtualbox" do |vb|
          vb.memory = "#{mem}"
          vb.cpus = 1
          vb.name = "#{minion}-#{project}"
      end

      minion_config.vm.box = "#{os}"
      minion_config.vm.box_version = "#{os_version}"
      minion_config.vm.hostname = "#{minion}-#{project}"
      minion_config.vm.network "private_network", ip: "#{ip}"

      minion_config.ssh.insert_key = false

      minion_config.vm.provision :salt do |salt|
        salt.minion_config = "saltstack/etc/#{minion}"
        salt.minion_key = "saltstack/keys/#{minion}.pem"
        salt.minion_pub = "saltstack/keys/#{minion}.pub"
        salt.install_type = "stable"
        salt.install_args = "3006.9"
        salt.verbose = true
        salt.colorize = true
        salt.bootstrap_script = "scripts/bootstrap-salt.sh"
        salt.bootstrap_options = "-P -c /tmp -x python3"
      end
    end
  end
end
