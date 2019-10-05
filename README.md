# roger-skyline-1

## V.1 VM Part
***hypervisor:*** VirtualBox; ***Linux OS:*** Debian(64-bit); size of the hard disk is 8.00 GB(VDI, fixed size);
Next you should run the virtual machine and specify the image of the OS - i downloaded `debian-10.1.0-amd64-netinst.iso` from https://www.debian.org/distrib/.
![specify_img](img/specify_img.png)

Then you need to set up you Debian, process is quite simple, but i'll pay some attention on `Partition disks` part. Choose `Partition method` as `manual` and next choose:
![partition_1](img/partition_1.png)

then:

![partition_2](img/partition_2.png)

go for `Create a new partition` and specify new partition size:

![partition_3](img/partition_3.png)

choose type and location (i choosed beggining); choose file system(i went for `/ - the root file system`):

![partition_4](img/partition_4.png)

i created 2 partitions: one `primary` with mout point on the `/ (root)` of OS and with 4.2GB capacity, second `logical` with mount point on the `/home` dir and 4.4GB of space:

![partition_5](img/partition_5.png)

then go for `Finish partitioning and write changes to disk`.
Finally, i did not install desktop envirinment; GRUB i accepted.
## V.2 Network and Security Part
### You must create a non-root user to connect to the machine and work.
Non-root login was created while setting up the OS. Just log in.
### Use sudo, with this user, to be able to perform operation requiring special rights.
First, we need to install `sudo`, what we can do only as root, so:
```
$ su
$ apt-get update -y && apt-get upgrade -y
$ apt-get install sudo vim -y
```
exit root mode:
```
$ exit
```
but now, if we'll try to use `sudo`, the OS will respond: `kseniia is not in the sudoers file. This incident will be reported`. That means we need to open `/etc/sudoers` file (again under the root). Don't forget to check rights on the file (must be writible!).
```
$ pwd
/etc
$ chmod +w sudoers
$ vim sudoers
```
add `username ALL=(ALL:ALL) ALL` to `# User priviliege specification` section:

![sudoers](img/sudoers.png)

### We don’t want you to use the DHCP service of your machine. You’ve got to configure it to have a static IP and a Netmask in \30.
First, go to VirtualBox settings -> Network -> in `Attached to` subsection change ***NAT*** on ***Bridged Adapter***; i like using `ifconfig`, that's why i'll install it (it's always possible to use `ip`):
```
$ sudo apt-get install net-tools
$ sudo ifconfig
```

![ifconfig](img/ifconfig.png)

As we see, the name of our `bridget adapter` is ***enp0s3***. Let's setup ***static ip*** (not dynamical) [* [How to setup a Static IP address on Debian Linux)](https://linuxconfig.org/how-to-setup-a-static-ip-address-on-debian-linux)].
***1.*** We should modify `/etc/network/interfaces` network config file (don't forget to`$ sudo chmod +w interfaces`):

![interfaces](img/interfaces.png)

[* [Файл настройки сети /etc/network/interfaces)](https://notessysadmin.com/fajl-nastrojki-seti)]
***2.*** Define your network interfaces separately within `/etc/network/interfaces.d/` directory. During the networking daemon initiation the `/etc/network/interfaces.d/` directory is searched for network interface configurations. Any found network configuration is included as part of the `/etc/network/interfaces`. So:
```
$ cd interfaces.d
$ sudo touch enp0s3
$ sudo vim enp0s3
```

![enp0s3](img/enp0s3.png)

next restart the network service:
```
$ sudo service networking restart
```
run `ifconfig` to see the result:

![ifconfig_res](img/ifconfig_res.png)

### You have to change the default port of the SSH service by the one of your choice. SSH access HAS TO be done with publickeys. SSH root access SHOULD NOT be allowed directly, but with a user who can be root.


## V.2 Web Part

## V.3 Deployment Part














