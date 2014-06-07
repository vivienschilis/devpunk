# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

CPUS = ENV['V_CPUS'] ? ENV['V_CPUS'].to_i : 2
RAM = ((ENV['V_RAM'] ? ENV['V_RAM'].to_i : 2) * 1024).to_s
INSTANCE_IP = "192.168.35.10"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # config.vm.box = "base-0.4.0"
  # config.vm.box_url =
  #   "base-0.4.0 http://paasta-boxes.s3.amazonaws.com/base-0.4.0-amd64-20131218-virtualbox.box"

  config.vm.box = "ubuntu-12.04"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.network :private_network, ip: INSTANCE_IP
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |vb|
    vb.gui = false

    vb.customize [ "modifyvm", :id, "--memory", RAM, "--cpus", CPUS]
    vb.customize [ "modifyvm", :id, "--natdnshostresolver1", "on"]

    vb.customize ["setextradata", :id,
      "VBoxInternal2/SharedFoldersEnableSymlinksCreate/bridge", 1
    ]
  end

  # Adds missing base-images dependency (will be fixed in 0.4.0)
  config.vm.provision :shell,
    inline: 'apt-get install -qy python-pycurl'

  config.vm.provision :ansible do |ansible|
    ansible.verbose = 'v'
    ansible.playbook = "site.yml"
    ansible.sudo = true
    ansible.extra_vars = { ansible_ssh_user: 'vagrant', ansible_connection: 'ssh', ansible_ssh_args: '-o ForwardAgent=yes'}
    ansible.raw_ssh_args = ['-o UserKnownHostsFile=/dev/null']
  end


end
