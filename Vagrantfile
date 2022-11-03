# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # By default FOLDER SYNCING is disabled.
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # To enable folder syncing, keep the line above as is and uncomment one of the following lines (rsync or NFS):
  #config.vm.synced_folder "<path to folder to sync>", "<mountpoint in vagrant box>", type: "rsync"
  #config.vm.synced_folder "../leapp-repository", "/leapp-repository", type: "nfs"
  # For rsync, it's advisable to setup auto-syncing: https://www.vagrantup.com/docs/cli/rsync-auto.html

  if ENV['SAT']
    puts "SAT env var was deprecated, please use SRC as a source for both RHEL7 and RHEL8"
    puts "Use 'rhsm' for CDN, 'sat' to you your Satellite instance or 'custom' to use your own custom repositories"
    exit
  end
  src = ENV['SRC'] || "rhsm"
  if src and src != "rhsm" && src != "sat" && src != "custom"
    puts "ERROR: The allowed values for SRC environment var are: rhsm (default), sat and custom"
    exit
  end
  framework_pr = ENV['FPR']
  repository_pr = ENV['RPR']

  config.vm.host_name = "leapp-"+Time.now.strftime("%Y%m%d%H%M%S")
  config.vm.box = "generic/rhel7"

  config.trigger.before :destroy do |trigger|
    trigger.name = "Unregistering from RHSM/Satellite. This can be ignored if the machine was not fully provisioned."
    trigger.on_error = :continue
    trigger.run_remote = {inline: "subscription-manager unregister"}
  end

  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048
    libvirt.cpus = 2
    libvirt.cpu_mode = 'host-passthrough'
    if ENV['SYSTEM_SESSION']
      libvirt.qemu_use_session = false
    end
  end

  config.vm.provision "setup", type: "ansible" do |ansible|
    ansible.playbook = "setup.yml"
    ansible.extra_vars = {
        framework_pr: framework_pr,
        repository_pr: repository_pr
    }
    ansible.extra_vars.merge!(src: src)
  end

  config.vm.provision :shell, inline: "echo Good job, now enjoy your box!"

  # When logging in with `vagrant ssh`, log in as root
  #config.ssh.username = 'vagrant'
  #config.ssh.password = 'vagrant'
  config.ssh.insert_key = true

  # Forward cockpit port
  config.vm.network "forwarded_port", guest: 9090, host: 9090
end
