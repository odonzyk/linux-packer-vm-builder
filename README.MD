# Linux VM builder
Automatically create, launch and provision new linux VM's with packer in a vSphere cluster and ansible included

## How it works
- You launch packer from the command line
- Packer fetches the debian ISO image from the internet and uploads it to vSphere server
- Packer creates and launches a new virtual machine in vSphere (see `debian-10.2.json`)
- Packer runs an unattended installation of a minimal system (see `http/preseed.cfg`)
- After installation packer does the remaining provisioning using ansible (see `provision-ansible/`) 

## Prepare your environment
- Install Packer (https://packer.io/)
- Install Packer Builder for VMware vSphere (https://github.com/jetbrains-infra/packer-builder-vsphere)
- Export AD_USERNAME and AD_PASSWORD environment variables of user with sufficient permissions to build VM
- Make sure the vsphere VM folder is created before trying to add a VM to it    

## Building a new VM
The example command below will create a VM with a 20GB disk, 2 CPU's and 2GB of RAM with hostname 'test-vm'
inside vsphere vm folder 'test-wouter'.

Note that you can override all variables defined at the bottom of `debian-10.2.json`.
```
packer build \
    -var 'cpus=2' -var 'memory=2048' -var 'disk_size_mb=20480' \
    -var 'hostname=test-vm' -var 'vsphere_vm_name=test-vm' \
    -on-error=ask debian-10.2.json
```

Tip: Launch a web console in vsphere to inspect packer installing debian.

## Accessing the VM after creating it
Access the VM over SSH using a user called 'debian' with password 'insecure', it has password less sudo access.

# References
Debian 10 (Buster) example preseed: https://www.debian.org/releases/buster/example-preseed.txt

## Thanks to-> !
Our Coworker Wouter Westenbrink, for the great summerize of search results out of the internet and his own adopt ideas for this reporitory!  
Hope we have a lot projects together in the future!
