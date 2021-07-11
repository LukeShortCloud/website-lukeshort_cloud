---
title: 2020-12-10
date: 2020-12-10 12:00:00
categories:
  - storage
tags:
  - benchmarking
  - filesystems
  - nas
  - storage
  - zfs
---

[Back to Linux blog.](../#linux)

# Benchmarking OpenZFS 2.0

{{< image src="../../images/blog/linux/2020-12-10-benckmarking-openzfs-2.png" alt="OpenZFS Benchmark Chart" set="fit" >}}

My OpenZFS benchmarks are in and the results are very enlightening!

The graph focuses on the tests using sequential zero read and writes. These seem to be more accurate than the urandom tests.

Let me cut to the chase:

- The new zstd compression in OpenZFS 2.0 is awesome. The results speak for themselves. I highly recommend enabling that.
- L2ARC cache is also a very compelling feature to use. From what I have seen from other benchmarks online, ZFS handles caching storage from a slower drive to a faster drive better than other file systems (such as Bcachefs).

Here are the full results for reference:

| L2ARC | Zstandard (zstd) Compression | Local or CIFS | `/dev/zero` or `/dev/urandom` | Write (MB/s) | Read (MB/s) |
| --- | --- | --- | --- | --- | --- |
| No | No | Local | zero | 161 | 124 |
| No | No | Local | urandom | 60.6 | 172 |
| No | Yes | Local | zero | 2200 | 7800 |
| No | Yes | Local | urandom | 60.9 | 171 |
| Yes | No | Local | zero | 196 | 187 |
| Yes | No | Local | urandom | 60.7 | 171 |
| Yes | Yes | Local | zero | 3300 | 7600 |
| Yes | Yes | Local | urandom | 61.0 | 172 |
| No | No | CIFS | zero | 138 | 188 |
| No | No | CIFS | urandom | 60.0 | 178 |
| No | Yes | CIFS | zero | 1700 | 250 |
| No | Yes | CIFS | urandom | 59.0 | 2100
| Yes | No | CIFS | zero | 128 | 60.0 |
| Yes | No | CIFS | urandom | 60.1 | 175 |
| Yes | Yes | CIFS | zero | 1500 | 2500 |
| Yes | Yes | CIFS | urandom | 60.0 | 174 |

Considerations for future benchmarking of OpenZFS:

- The virtual machine used for testing may be missing an optimization for /dev/urandom generation. Across the board, the write speeds from /dev/urandom were at 60 MB/s. All of those write tests can be assumed invalid for now. I may revisit this in the future.
- CIFS always exagerates its speeds. I’m unsure how to overcome this. It’s likely to be a mount option or two.
- CIFS should be tested as a mount on an external machine.
- L2ARC cache tests were not conclusive enough. For future tests, `arcstat` and `arcsummary` should be used to monitor caching. Using a large set of files may help as well.

**Test Setup**

Virtual machine specs:

- Hypervisor: KVM
- Debian 10.7
- 8 vCPUs (4 cores, 8 threads) provided by an AMD Ryzen 3950X
- 32 GB DDR4 RAM 3200 MHz
- Storage:
    - SSD 80 GB passthrough
        - Speed rated at 500 MB/s (read and write
    - HDD 8 TB passthrough
        - Speed rated at 185 MB/s (read and write)
    - Settings:
        - Cache mode = none
        - IO mode = native
- Network = VirtIO bridge

ZFS pool created on my 8 TB HDD:

```
$ sudo zpool create zfsbenchmark sdb
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0 74.5G  0 disk 
sdb      8:16   0  7.3T  0 disk 
├─sdb1   8:17   0  7.3T  0 part 
└─sdb9   8:25   0    8M  0 part 
vda    254:0    0   80G  0 disk 
└─vda1 254:1    0   80G  0 part /
```

Zstandard compression setup on a different dataset:

```
$ sudo zfs create -o casesensitivity=mixed -o compression=zstd zfsbenchmark/zstd
```

CIFS setup:

```
$ sudo apt install samba cifs-utils
$ sudo zfs set sharesmb=on zfsbenchmark
$ sudo useradd zfoo
$ sudo smbpasswd -a zfoo
New SMB password: bar
Retype new SMB password: bar
Added user zfoo.
$ sudo mkdir /cifs/
$ sudo chown -R zfoo /cifs
$ sudo mount -t cifs -o username=zfoo,password=bar //127.0.0.1/zfsbenchmark /cifs/
$ sudo su - zfoo
```

Add a SSD as a cache front of the HDD. This is also known as a L2ARC cache. This was only done after all of the non-L2ARC benchmarks were completed:

```
$  sudo zpool add zfsbenchmark cache sda
$  lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0 74.5G  0 disk 
├─sda1   8:1    0 74.5G  0 part 
└─sda9   8:9    0    8M  0 part 
sdb      8:16   0  7.3T  0 disk 
├─sdb1   8:17   0  7.3T  0 part 
└─sdb9   8:25   0    8M  0 part 
vda    254:0    0   80G  0 disk 
└─vda1 254:1    0   80G  0 part /
```

How were the benchmarks performed? Before every test, I ran `sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches` to sync all writes to the disk and flush out the swap. The tests were preformed using similar commands to write and read 10 GB files:

```
# Zero
## Write
$ sudo dd if=/dev/zero of=/zfsbenchmark/zero.img bs=1M count=10000
$ sudo dd if=/dev/zero of=/zfsbenchmark/zstd/zero.img bs=1M count=10000
$ sudo dd if=/dev/zero of=/cifs/zero.img bs=1M count=10000
## Read
$ sudo dd if=/zfsbenchmark/zero.img of=/dev/null bs=1M
$ sudo dd if=/zfsbenchmark/zstd/zero.img of=/dev/null bs=1M
$ sudo dd if=/cifs/zero.img of=/dev/null bs=1M
# Random
$ sudo dd if=/dev/urandom of=/zfsbenchmark/nonzero.img bs=1M count=10000
$ sudo dd if=/dev/urandom of=/zfsbenchmark/zstd/nonzero.img bs=1M count=10000
$ sudo dd if=/dev/urandom of=/cifs/nonzero.img bs=1M count=10000
## Read
$ sudo dd if=/zfsbenchmark/nonzero.img of=/dev/null bs=1M
$ sudo dd if=/zfsbenchmark/zstd/nonzero.img of=/dev/null bs=1M
$ sudo dd if=/cifs/nonzero.img of=/dev/null bs=1M
```

Side note: /dev/urandom is NOT secure. I only used it for testing because, in theory, it's supposed to be faster. Stick to /dev/random for generating random entropy for use in production applications.

So long for now!
