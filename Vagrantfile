Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  config.vm.define "ovirt" do |ovirt|
    ovirt.vm.hostname = "ovirt"
    ovirt.vm.provider "libvirt" do |libvirt|
        libvirt.cpus = 8
        libvirt.memory = 12288
        libvirt.cpu_mode = "host-passthrough"
        libvirt.storage :file, :size => '100G'
    end
  end

  config.vm.provision :ansible_local do |ansible_local|
    ansible_local.playbook = "site.yml"
    ansible_local.become = true
  end

end
