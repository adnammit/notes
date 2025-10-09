# Home Server

## TODO
* have an expansion plan -- starting with mirrored vdevs, so you can add more mirrored vdevs later
* cache drive?
* optical drive?
* Energy monitoring: PGE avg cost is $0.16/kwh
	* home server: $0.73/week - $3.16/month
	* PC and desk setup: $8.44/month

## Concepts
* **JBOD**: "just a bunch of discs" - basically what you do on your pc. nothing is backed up, and you get 100% of your space. do this for your pc when important stuff is backed up on the cloud, but don't do this for nas
* **software RAID** (i.e. zfs) vs **hardware RAID**
* **HBA** vs **RAID controller**: hba is just hardware that allows you to plug drives into your computer - the OS will use software RAID to manage the drives. RAID controllers create an array which is then presented to the OS as a single drive
* **CMR** vs **SMR**: you want CMR drives - they can be rewritten, generally better for nas
* **SAS** vs **SATA**: SAS is a different interface. SAS interfaces are backwards compatible with SATA drives but not the other way around
* **hypervisor**: a virtual machine monitor (VMM), software that creates and runs VMs. allows guest VMs to share the same hardware resources (memory, processing)

# Proxmox

## Setup Notes
* [PVE Mods](https://github.com/Meliox/PVE-mods)
* installed:
	* PVE Mods: Node Sensor Readings
		* includes `lm-sensors` - run `sensors` to see temps immediately
		* may need to install kernel module `drivetemp` to see hdd readings - see PVE-mods readme
* [adding backports](https://backports.debian.org/Instructions/) (not-yet-supported packages)
	* updated smartctl to 7.4 bc i needed the new 'farm' feature to check those bogus seagate disks
* setting up [fancontrol](https://wiki.joeplaa.com/tutorials/how-to-install-and-configure-fancontrol-pc)
* you'll want to make sure proxmox is regularly updated but there are issues if you're not a paid customer - see [this video](https://youtu.be/5AumO9AKKB0?si=3h8AR_b2BB5ySuid) for a workaround for ` command 'apt-get update' failed: exit code 100`
* check out [community scripts](https://community-scripts.github.io/ProxmoxVE/) for easy ways to customize and update proxmox, like running the `post install` script to get rid of nags etc

## Quick Commands
```sh
# see what needs to be updated
apt update
# proxmox-specific upgrade
pveupgrade
# clean up
apt autoremove
# reboot if you installed a new kernel
```


* disk management
```sh
# list all your disks and partitions
ls -la /dev/disk/by-id/
# nice overview of all drives
duf
# get uuids for all drives
blkid
```

* bogus HDD stuff
```sh
# view smart disk data
smartctl -a /dev/sd{a,b}

# check seagate farm data
smartctl -l farm /dev/sd{a,b}

# bad amazon hdds, bad!
smartctl -l farm /dev/sda > seagate_farm_output_ZA1JXA4Z.txt
smartctl -l farm /dev/sdb > seagate_farm_output_ZA1JX8VA.txt
```

* you can ssh into your home server:
```sh
ssh root@<your-server-ip>
```

## Overview
* Proxmox is an open-source virtualization platform that combines KVM (Kernel-based Virtual Machine) and LXC (Linux Containers) for managing virtualized environments
* a type one or "bare metal" hypervisor (vs type two which run on top of the OS)
* provides a web-based interface for easy management of VMs, containers, storage, and networking
* **datacenter** is the top-level object that contains all resources, including nodes, storage, and VMs
	* user permissions, backup, firewalls, and storage can all be managed at the datacenter level
	* you can search for specific resources across the whole datacenter
	* **summary** has a good overview of resource usage and performance metrics
* **node** is a physical server that runs the Proxmox VE software and hosts VMs and containers
* **localnetwork** is the VM running proxmox on the node - allows your VMs to talk to your LAN and internet
* **local storage** is the storage that is physically attached to the node and can be used by VMs and containers - typically contains disk images, backups and container data
* **local-zfs storage**: if you selected zfs, this storage will also be included in the UI. includes snapshots and rollbacks


## VMs and LXCs
* VMs (Virtual Machines) are fully isolated environments that run their own operating systems and applications
* LXCs (Linux Containers) are lightweight, share the host OS kernel, and are more efficient in terms of resource usage
* Use VMs for applications that require full isolation or different OS versions
* Use LXCs for lightweight applications or when you need to run multiple instances of the same OS


## Data
* data can be managed at the node level or at the datacenter level
* **node**: the physical devices available
* **datacenter**: what's used and who can use it
* **filesystem**: the way data is organized and stored on a disk. ex: xfs, ZFS, btrfs, ext4
* **storage type**: the way that proxmox interacts with the storage. ex: how VM disk, backup, iso files are managed
* storage types can be **block level** or **file level**
	* in block-level storage, you can only store VM disk or the container disk, not individual files
		* blocks are a sequence of bytes with a hash value address
		* data is stored without metadata
		* used in structured workloads
		* use-case examples: email servers, storage for VMs, databases, RAID block storage
	* in file-level storage, you can store individual files and directories
		* files and metadata are stored in a hierarchical structure
		* the data appears the same to the system writing it as to the system/user reading it
		* use-case examples: file servers, LAN, web servers, backup storage
* storage types at the node level:
	* **directory**: file-level storage; used as a regular folder on a mounted filesystem like xfs or ext4
		* ext4 is good for general use, xfs is good for large files like backups
		* with file-level storage, you can store a lot more stuff like backups, ISOs, images - so it is a lot more flexible than block-level storage
	* **lvm**: logical volume management: block-level storage with good performance, but not many features
	* **lvm thin**: a more advanced form of LVM that allows for thin provisioning and better space utilization
		* **thin provisioning** means that you can *overprovision* your VMs: give them 5TB even if you only have 4TB of physical storage
		* additionally, the VM will only take what it needs - if it is allocated 5TB but only needs 2TB, it will only use 2TB whereas in other storage types, it would use the full 5TB
	* **zfs**: can act as block level or file-level storage. a combined file system and logical volume manager designed for high storage capacities. offers volume management, snapshots, compression, and data integrity features
		* in proxmox, if you "Add storage" when you create it, it will be added as block storage
		* to create file level storage with zfs, create without "Add storage" and use the cli to add it 

### Datacenter
* when you are defining storage at the node level, you are provisioning your physical drives
* data is not provisioned at the datacenter level, but it can be managed there
	* lvm, lvm thin, directory and btrfs can all be mapped at the datacenter level
* so: you create storage at the node level and you use it and external data sources at the datacenter level
* external storage can include SMB and NFS
* datacenter level storage can be shared across nodes


### Disks
* partitioning is independent of formatting
* you can have multiple partitions on a single disk
* each partition can have its own filesystem
* common partitioning schemes include MBR and GPT


# NAS
* OS: OMV or TrueNAS scale
* you're gonna wanna have apps: https://selfh.st/apps/
* photo management: check out amazon photos


# ZFS
* [pool layout with zfs](https://klarasystems.com/articles/choosing-the-right-zfs-pool-layout/)
* **zfs** is a combined file system and logical volume manager designed for high storage capacities
* zfs can act as block level or file level storage
* zfs is feature-rich but demands a lot of memory
* zfs uses virtual devices (**vdevs**) to create **pools**
* **pools** are the highest level of abstraction in ZFS. pools contain:
	* **vdevs** which are the building blocks of ZFS storage
	* **datasets** which are the actual filesystems that you interact with
	* volumes (**zvols**) which are block devices that can be used like a physical disk
* **vdevs** are the building blocks of ZFS storage, and can be made up of one or more physical devices
* **resilvering** is the process of rebuilding a vdev after a drive failure
	* the speed of resilvering is determined by the speed of the drives in the vdev, but also the amount of data - it takes the same amount of time to resilver a 10T drive with 2T of data as it does to resilver a 2T drive with 2T of data
	* therefore resilvering can be sped up by using more, smaller hard drives where the amount of data on any one drive isn't too large
	* mirrors also resilver faster than RAIDZ and each RAIDZ level takes longer (RAIDZ1 < RAIDZ2 < RAIDZ3)
* **compression types**
	* **lz4**: fast compression, low CPU usage
	* **gzip**: higher compression ratios, more CPU usage
	* **zle**: zero-length encoding, efficient for sparse data
* **ashift**: ????

## Resilience
* combining multiple vdevs into one pool effectively creates a giant striped drive, but in this case you have no redundancy, and even worse, bc of striping, if one drive fails, you lose everything
* additionally, it's not just the vdev that is at risk -- **a single vdev failing makes its whole pool fail**
* therefore each vdev must provide its own redundancy - for real redundant data storage, you need to choose between mirrors, RAID-Z or dRAID vdev types
* when you have a lot of drives, you want to avoid creating "wide vdevs" -- you can theoretically have a vdev with many drives, but as you add drives the risk of a drive failing increases, so it tends to be better to have more, smaller vdevs

## Mirrors
* **mirrors** 
* PROS:
	* mirrors provide the best IOPS (the number of read and/or write operations that can be performed per second)
	* the more vdevs in the pool, the more IOPS that are available
	* resilvering is faster than RAID-Z
* CONS: require more drives to achieve the same amount of usable storage as RAID-Z as everything is duplicated

## RAID-Z
* **RAID-Z** is essentially RAID5, but with some improvements
* RAIDZ relies on the concept of **parity** to provide redundancy. Parity is a mathematical calculation that can be used to reconstruct data if a drive fails, so in RAIDZ1 with three drives, two drives will contain data and one will contain parity. if any one of them fails, the data can be reconstructed from the other two
* RAID-Z vdevs require at least three disks but provide more usable space than mirrors. They also solve the RAID-5 "write hole problem" in which data and parity can become inconsistent after a crash
* RAID-Z can have single, double or triple parity (RAID-Z1, RAID-Z2, RAID-Z3) which means it can withstand one, two or three drive failures respectively

## Caches
* **L2ARC**: you can set up a read cache using an L2ARC device 
	* this is a separate drive that stores frequently accessed data to speed up read operations on slower drives (i.e. you would use an SSD to cache reads from a pool of HDDs)
	* each pool can have one read cache
* **log write cache**: you can set up a write cache using a ZIL (ZFS Intent Log) device (SLOG or Separate Intent LOG SSD)
	* this is a separate drive that stores write operations to speed up write operations on slower drives
	* each pool can have one write cache

## Performance
* metrics for performance include: bandwidth, iops
* **IOPS**: the number of read and/or write operations that can be performed per second
* typically write speed is faster than read speed for HDDs and SSDs
* using an ARC cache can substantially speed up your read speeds (good for NAS)
* more drives == faster speeds
* RAID-Z is slower than mirrors but provides more usable space


## ZFS Config for OMV
* [mirrored vs raidz vdevs](https://jrs-s.net/2015/02/06/zfs-you-should-use-mirror-vdevs-not-raidz/)
* option 1: one vdev with 3 or 4 drives in RAID-Z1
	* more space, less IOPS
	* how to extend?
* option 2: two 2-way mirrored vdevs (or start with one 2-way mirrored vdev)
	* better IOPS, less space
	* less stressful to rebuild a failed disk


# Plex
* Picture quality will likely bottleneck at mobo: 4k @24hz (see below) 
* Router spec:
	* your router should support at least gigabit connections on each of its Ethernet ports to enable super-fast transfer speeds. This is especially important for applications like remote file access and streaming high-resolution media (such as 4K movies).
	* If your current router lacks sufficient gigabit ports or features, consider upgrading to one with robust QoS (Quality of Service) features. These allow you to prioritize traffic to and from your NAS, improving overall performance
* mobo graphics:
	Onboard Graphics
	Integrated Graphics Processor-Intel® HD Graphics support
	1 x DVI-D port, supporting a maximum resolution of 1920x1200@60 Hz
	* The DVI-D port does not support D-Sub connection by adapter.
	2 x HDMI ports, supporting a maximum resolution of 4096x2160@24 Hz
	* Support for HDMI 1.4 version.

# Network

## Ethernet Bandwidth
* current ethernet cables are Cat6 (gigabit, 150MHz)
* PC mobo supports 2.5g ethernet
* NAS mobo only supports gigabit ethernet, so until/unless you upgrade your NAS, this will be your bottleneck anyway




* [omv/plex setup](https://youtu.be/aEzo_u6SJsk?si=goF6eDwsf4vOM6Z9) - this will only work if you read-only
* [omv/plex WITH write](https://youtu.be/CFhlg6qbi5M?si=ZmBlhKkvnV_U8d_0)
* need a drive for the omv install i think?
* files should live in OMV and be shared to plex
* configure a shared drive/shared drive user in ovm
* plex should be unprivileged and it won't be able to access the plex drive
* gpt vs zfs vs smb (partition vs filesystem vs sharing)
* does it make any sense to partition drives for mirroring? - probably not
* what about caching?
* does this make sense:
	* one pool with one mirrored vdev, 2 discs
	* when mirroring, why have more than one vdev per pool? doesn't that put the pool at risk?



* [zfs pool setup](https://youtu.be/oSD-VoloQag?si=ytBRxqpB14zS884h)
	* mirror
	* compression: lz4
	* ashift: 12
	* more, smaller vdevs
* [omv setup](https://youtu.be/vTLxIPDty_Y?si=9oCqgVYI2ZCY-QJe)
