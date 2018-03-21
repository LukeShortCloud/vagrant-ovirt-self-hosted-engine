# vagrant-ovirt-self-hosted-engine

This Vagrantfile and Ansible Playbook provides a way to easily setup an all-in-one lab environment for oVirt.

## Requirements

* KVM+QEMU with [nested virtualization enabled](https://github.com/ekultails/rootpages/blob/master/src/virtualization.rst#nested-virtualization)
* [vagrant-libvirt](https://github.com/vagrant-libvirt/vagrant-libvirt)

## Usage

Create the virtual machine.

```
$ vagrant up
```

After the Playbook is completed setting up the environment, log into the virtual machine to finish the installation.

```
$ vagrant ssh
$ sudo hosted-engine --deploy
```

Use the default values for everything. Be sure to set the NFS export with the Vagrant virtual machine's IP address.

oVirt 4.1:
```
          Please specify the storage you would like to use (glusterfs, iscsi, fc, nfs3, nfs4)[nfs3]: nfs4
          Please specify the full shared storage connection path to use (example: host:/path): <VM_IP>:/exports/data
```
```
          Please provide the FQDN for the engine you would like to use.
          This needs to match the FQDN that you will use for the engine installation within the VM.
          Note: This will be the FQDN of the VM you are now going to create,
          it should not point to the base host or to any other existing machine.
          Engine FQDN:  []: engine.example.com
```

oVirt 4.2:

```
          Please specify the storage you would like to use (glusterfs, iscsi, fc, nfs)[nfs]: nfs
          Please specify the nfs version you would like to use (auto, v3, v4, v4_1)[auto]: v4_1
          Please specify the full shared storage connection path to use (example: host:/path): <VM_IP>:/exports/data
```
```
          Please provide the FQDN for the engine you would like to use.
          This needs to match the FQDN that you will use for the engine installation within the VM.
          Note: This will be the FQDN of the VM you are now going to create,
          it should not point to the base host or to any other existing machine.
          Engine FQDN:  []: engine.example.com
```

### Variables

* ovirt_version_major = The major version of oVirt. This should be left at 4.
* ovirt_version_minor = The minor version of oVirt to install.
* enable_branch_development = Install the latest development/snapshot packages corresponding to the version defined above.
* enable_branch_master = Install the latest packages for the next upcoming minor version of oVirt. This will ignore the previous `ovirt_version_*` settings.
* nfs_export_dir = The directory to create a network share for and to mount an additional volume to.
* hostname_hypervisor = The hostname for the Vagrant virtual machine.
* hostname_ovirt_engine = The hostname to use for the oVirt Engine that will be nested virtualized inside the Vagrant virtual machine. This fully qualified domain name (FQDN) should not already be in use by any other nodes.
* storage_file_system = The file system to use for the volume that will be created and mounted to the `nfs_export_dir`.

## Known Issues

In oVirt 4.2, the installation might fail due to what looks to be an ASCII encoding issue. The real problem here is that there was an issue with the specified NFS mount. The oVirt >= 4.3 release should fix the parsing of the error.

```
[ INFO  ] TASK [Copy configuration files to the right location on host]                                                                   
[ INFO  ] TASK [Copy configuration archive to storage]
[ ERROR ]  [WARNING]: Failure using method (v2_runner_on_failed) in callback plugin                                                       

[ ERROR ] (<ansible.plugins.callback.1_otopi_json.CallbackModule object at 0x190d650>):                                                   

[ ERROR ] 'ascii' codec can't encode character u'\u2018' in position 494: ordinal not in                                                  

[ ERROR ] range(128)

[ ERROR ] Failed to execute stage 'Closing up': Failed executing ansible-playbook
```

References:

* http://lists.ovirt.org/pipermail/users/2018-January/086631.html
* https://bugzilla.redhat.com/show_bug.cgi?id=1533500

## License

Apache 2.0
