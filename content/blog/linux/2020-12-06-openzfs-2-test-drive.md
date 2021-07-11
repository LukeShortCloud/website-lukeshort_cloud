---
title: 2020-12-06
date: 2020-12-06 12:00:00
categories:
  - storage
tags:
  - filesystems
  - nas
  - notes
  - rootpages
  - storage
  - tutorial
  - zfs
---

[Back to Linux blog.](../#linux)

# Taking OpenZFS 2.0 out for a Spin!

{{< image src="../../images/blog/linux/2020-12-06-openzfs-2-test-drive.jpg" alt="WD Western Digital easystore external hard drive" set="fit" >}}

OpenZFS 2 has arrived! The open source project for providing the ZFS file system on BSD, Linux, Windows, and (soon-to-be) macOS had a major release this past week.

I'm a huge fan of Btrfs (or Buttery-F-S, as I like to call it). For the longest time, Btrfs was the king of Linux file systems. Some may say that it's unstable because companies like Red Hat have removed support for it. However, Red Hat has 0 engineers working on the project. On the other hand, SUSE has a full department that works diligently on this wonderful file system. Once upon a time, it was not stable. I have experienced many file systems corruptions with Btrfs back in the day when it was young. Things have changed and nowadays it's a real contender for production workloads. Where does ZFS fit into all of this? Well, it could be argued that Btrfs has spent its entire life trying to catch up with ZFS in terms of features and performance.

In a world where patents and licesning do not matter, ZFS takes the crown. It's literally the perfect file system that includes everything plus the kitchen sink! Copy-on-write, datasets/subvolumes, compression, snapshots, live/hot FSCK, RAM cache, cache drives, data deduplication, software RAID support, and more!

I was excited to hear when OpenZFS 2.0 was released but, at the same time, confused at the lack of packaging. I have been waiting a long time for this release, so let's dive right into the source now and build it from scratch! These are [my notes on how to build and install OpenZFS 2](https://github.com/ekultails/rootpages/blob/master/src/storage/file_systems.rst#installation-source).

To test this, I created a Debian 10 virtual machine with two additional QCOW2 images attached. Imagine, if you will, that ``/dev/vdb`` is our SSD and that ``/dev/vdc`` is our HDD. The SSD will be used as a cache drive for the HDD. This is a trial of how I will end up re-configuring my home file server using Debian and ZFS.

**ZFS Pool Creation**

These are my existing partitons:

```
$ sudo lsblk
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
vda    254:0    0  80G  0 disk 
└─vda1 254:1    0  80G  0 part /
vdb    254:16   0   1G  0 disk 
vdc    254:32   0  10G  0 disk 
```

Create the main pool using the HDD:

```
$ sudo zpool create examplepool vdc
```

Add the SSD as a L2ARC (cache) drive:

```
$ sudo zpool add examplepool cache vdb
```

Here are our partitions now:

```
$ sudo lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
vda    254:0    0   80G  0 disk 
└─vda1 254:1    0   80G  0 part /
vdb    254:16   0    1G  0 disk 
├─vdb1 254:17   0 1014M  0 part 
└─vdb9 254:25   0    8M  0 part 
vdc    254:32   0   10G  0 disk 
├─vdc1 254:33   0   10G  0 part 
└─vdc9 254:41   0    8M  0 part 
```

Let's examine the status of the pool:

```
$ sudo zpool status
  pool: examplepool
 state: ONLINE
config:

	NAME         STATE     READ WRITE CKSUM
	examplepool  ONLINE       0     0     0
	  vdc        ONLINE       0     0     0
	cache
	  vdb        ONLINE       0     0     0

errors: No known data errors
```

**Dataset Creation**

Create a dataset (similar in concept to Btrfs subvolumes). Let's also set case insensitive so this will work as a CIFS share on Windows along with the newly anticipated zstd compression:

```
$ sudo zfs create -o casesensitivity=mixed -o compression=zstd examplepool/exampledataset
```

Check out the new dataset. Notice how pools and datasets will automatically mount themselves:

```
$ sudo zfs list
NAME                        USED  AVAIL     REFER  MOUNTPOINT
examplepool                 178K  9.20G       24K  /examplepool
examplepool/exampledataset   24K  9.20G       24K  /examplepool/exampledataset
```

**Compression**

How do we know if compression is working? Let's create a large file full of zeroes:

```
$ sudo dd if=/dev/zero of=/examplepool/exampledataset/zero.img bs=1M count=100
```

Notice how zstd compresses that 100MB file down to basically nothing! Granted, it's easy to compress an endless supply of zeroes. You won't be able to get infinite storage by using compression. ;-)

```
$ sudo zfs list
NAME                         USED  AVAIL     REFER  MOUNTPOINT
examplepool                  189K  9.20G       24K  /examplepool
examplepool/exampledataset    24K  9.20G       24K  /examplepool/exampledataset
```

How about a test with a more "real life" example. Let's create another 100 MB file but this time it will be full of random data:

```
$ sudo dd if=/dev/urandom of=/examplepool/exampledataset/nonzero.img bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 2.35945 s, 44.4 MB/s
```

Seeing is believing! We have saved 7 MB of space thanks to compression. The space savings will vary upon each individual file/inode:

```
$ sudo zfs list
NAME                         USED  AVAIL     REFER  MOUNTPOINT
examplepool                 93.2M  9.11G       24K  /examplepool
examplepool/exampledataset  93.1M  9.11G     93.1M  /examplepool/exampledataset
```

**Samba/CIFS**

Samba time! *Cue catchy dance music.* ZFS supports settings up NFS and Samba shares. Let's try Samba since it's a bit more complex:

```
$ sudo apt install samba
$ sudo zfs set sharesmb=on examplepool/exampledataset
$ sudo useradd foo
$ sudo chown -R foo.foo /examplepool/exampledataset/
$ sudo smbpasswd -a foo
New SMB password: foobar
Retype new SMB password: foobar
Added user foo.
```

Let's verify the CIFS mount works properly:

```
$ sudo apt install cifs-utils
$ sudo mount -t cifs -o username=foo,password=foobar //127.0.0.1/examplepool_exampledataset /mnt
$ ls -lah /mnt
total 103M
drwxr-xr-x  2 root root    0 Dec  4 06:54 .
drwxr-xr-x 19 root root 4.0K Dec  4 06:38 ..
-rwxr-xr-x  1 root root 100M Dec  4 06:54 nonzero.img
-rwxr-xr-x  1 root root 100M Dec  4 06:51 zero.img
```

**Closing Remarks**

Pools, datasets, compression, CIFS with ZFS, and ZFS on Linux: it's true. All of it. It works on Linux! Don't take my word for it, go try it for yourself! :-)

Stay tuned for a future article on ZFS performance tuning and benchmarking!
