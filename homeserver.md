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

## Proxmox

### Setup Notes
* [PVE Mods](https://github.com/Meliox/PVE-mods)
* installed:
	* PVE Mods: Node Sensor Readings
		* includes `lm-sensors` - run `sensors` to see temps immediately
		* may need to install kernel module `drivetemp` to see hdd readings - see PVE-mods readme
* [adding backports](https://backports.debian.org/Instructions/) (not-yet-supported packages)
	* updated smartctl to 7.4 bc i needed the new 'farm' feature to check those bogus seagate disks
* setting up [fancontrol](https://wiki.joeplaa.com/tutorials/how-to-install-and-configure-fancontrol-pc)
* you'll want to make sure proxmox is regularly updated but there are issues if you're not a paid customer - see [this video](https://youtu.be/5AumO9AKKB0?si=3h8AR_b2BB5ySuid) for a workaround for ` command 'apt-get update' failed: exit code 100`


### Quick Commands
```sh
# view smart disk data
smartctl -a /dev/sd{a,b}

# check seagate farm data
smartctl -l farm /dev/sd{a,b}

# bad amazon hdds, bad!
smartctl -l farm /dev/sda > seagate_farm_output_ZA1JXA4Z.txt
smartctl -l farm /dev/sdb > seagate_farm_output_ZA1JX8VA.txt
```



## NAS
* OS: OMV or TrueNAS scale
* you're gonna wanna have apps: https://selfh.st/apps/
* photo management: check out amazon photos


## ZFS
* [pool layout with zfs](https://klarasystems.com/articles/choosing-the-right-zfs-pool-layout/)
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

### Resiliance
* combining multiple vdevs into one pool effectively creates a giant striped drive, but in this case you have no redundancy, and even worse, bc of striping, if one drive fails, you lose everything
* additionally, it's not just the vdev that is at risk -- **a single vdev failing makes its whole pool fail**
* therefore each vdev must provide its own redundancy - for real redundant data storage, you need to choose between mirrors, RAID-Z or dRAID vdev types
* when you have a lot of drives, you want to avoid creating "wide vdevs" -- you can theoretically have a vdev with many drives, but as you add drives the risk of a drive failing increases, so it tends to be better to have more, smaller vdevs

### Mirrors
* **mirrors** 
* PROS:
	* mirrors provide the best IOPS (the number of read and/or write operations that can be performed per second)
	* the more vdevs in the pool, the more IOPS that are available
	* resilvering is faster than RAID-Z
* CONS: require more drives to achieve the same amount of usable storage as RAID-Z as everything is duplicated

### RAID-Z
* **RAID-Z** is essentially RAID5, but with some improvements
* RAIDZ relies on the concept of **parity** to provide redundancy. Parity is a mathematical calculation that can be used to reconstruct data if a drive fails, so in RAIDZ1 with three drives, two drives will contain data and one will contain parity. if any one of them fails, the data can be reconstructed from the other two
* RAID-Z vdevs require at least three disks but provide more usable space than mirrors. They also solve the RAID-5 "write hole problem" in which data and parity can become inconsistent after a crash
* RAID-Z can have single, double or triple parity (RAID-Z1, RAID-Z2, RAID-Z3) which means it can withstand one, two or three drive failures respectively

### Caches
* **L2ARC**: you can set up a read cache using an L2ARC device 
	* this is a separate drive that stores frequently accessed data to speed up read operations on slower drives (i.e. you would use an SSD to cache reads from a pool of HDDs)
	* each pool can have one read cache
* **log write cache**: you can set up a write cache using a ZIL (ZFS Intent Log) device (SLOG or Separate Intent LOG SSD)
	* this is a separate drive that stores write operations to speed up write operations on slower drives
	* each pool can have one write cache

### Performance
* **IOPS**: the number of read and/or write operations that can be performed per second
* typically write speed is faster than read speed for HDDs and SSDs
* using an ARC cache can substantially speed up your read speeds (good for NAS)
* more drives == faster speeds
* RAID-Z is slower than mirrors but provides more usable space


### ZFS Config for OMV
* [mirrored vs raidz vdevs](https://jrs-s.net/2015/02/06/zfs-you-should-use-mirror-vdevs-not-raidz/)
* option 1: one vdev with 3 or 4 drives in RAID-Z1
	* more space, less IOPS
	* how to extend?
* option 2: two 2-way mirrored vdevs (or start with one 2-way mirrored vdev)
	* better IOPS, less space
	* less stressful to rebuild a failed disk


## Plex
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

## Network

### Ethernet Bandwidth
* current ethernet cables are Cat6 (gigabit, 150MHz)
* PC mobo supports 2.5g ethernet
* NAS mobo only supports gigabit ethernet, so until/unless you upgrade your NAS, this will be your bottleneck anyway


