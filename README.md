# egeneralov.zfs

ZFS installation for debian

## Requirements

- debian
- 5.x kernel (tested on 5.5.2)

## Usage

Run ansible role.

```yaml
- hosts: servers
  remote_user: root
  roles:
     - egeneralov.zfs
```

Find your disks by path:

```bash
root@server:~# ls -lha /dev/disk/by-path
lrwxrwxrwx 1 root root   9 Feb  8 01:30 pci-0000:00:1f.2-ata-3 -> ../../sdb
lrwxrwxrwx 1 root root   9 Feb  8 01:30 pci-0000:00:1f.2-ata-4 -> ../../sdc
```

Create pool:

```bash
root@server:~# zpool create -m /data/ tank pci-0000:00:1f.2-ata-3 pci-0000:00:1f.2-ata-4
root@server:~# zpool list
NAME   SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
tank     9G   480K  9.00G        -         -     0%     0%  1.00x    ONLINE  -
root@server:~# zpool status
  pool: tank
 state: ONLINE
  scan: none requested
config:

	NAME                      STATE     READ WRITE CKSUM
	tank                      ONLINE       0     0     0
	  pci-0000:00:1f.2-ata-3  ONLINE       0     0     0
	  pci-0000:00:1f.2-ata-4  ONLINE       0     0     0
```

Also you can enable compresion & deduplication

```bash
root@server:~# zfs set compression=lz4 tank
root@server:~# zfs set dedup=on tank
```

You can find more info at FreeBSD Handbook [Chapter 19. The Z File System (ZFS)](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/zfs.html).

License
-------

MIT

Author Information
------------------

Eduard Generalov <eduard@generalov.net>
