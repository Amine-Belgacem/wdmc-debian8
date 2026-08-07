# WDMC Debian 8 Install Log

Raw terminal log from a successful Debian Jessie restore on WD My Cloud Gen1.

```text
root@mint:/# parted /dev/sdd
GNU Parted 3.6
Using /dev/sdd
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print                                                            
Model: WDC WD20 EFRX-68EUZN0 (scsi)
Disk /dev/sdd: 2000GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start  End  Size  File system  Name  Flags

(parted) mklabel gpt                                                      
Warning: The existing disk label on /dev/sdd will be destroyed and all data on
this disk will be lost. Do you want to continue?
Yes/No? yes                                                               
(parted) mkpart primary 528M 2576M                                        
(parted) mkpart primary 2576M 4624M                                       
(parted) mkpart primary 16M 528M                                          
(parted) mkpart primary 4828M 100%                                        
(parted) mkpart primary 4624M 4724M                                       
(parted) mkpart primary 4724M 4824M                                       
(parted) mkpart primary 4824M 4826M                                       
(parted) mkpart primary 4826M 4828M                                       
(parted) print                                                            
Model: WDC WD20 EFRX-68EUZN0 (scsi)
Disk /dev/sdd: 2000GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     Flags
 3      15.7MB  528MB   513MB                primary
 1      528MB   2576MB  2048MB               primary
 2      2576MB  4624MB  2048MB               primary
 5      4624MB  4724MB  99.6MB               primary
 6      4724MB  4824MB  101MB                primary
 7      4824MB  4826MB  1049kB               primary
 8      4826MB  4828MB  2097kB               primary
 4      4828MB  2000GB  1996GB               primary

(parted) set 1 raid on                                                    
(parted) set 2 raid on                                                    
(parted) print                                                            
Model: WDC WD20 EFRX-68EUZN0 (scsi)
Disk /dev/sdd: 2000GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     Flags
 3      15.7MB  528MB   513MB                primary
 1      528MB   2576MB  2048MB               primary  raid
 2      2576MB  4624MB  2048MB               primary  raid
 5      4624MB  4724MB  99.6MB               primary
 6      4724MB  4824MB  101MB                primary
 7      4824MB  4826MB  1049kB               primary
 8      4826MB  4828MB  2097kB               primary
 4      4828MB  2000GB  1996GB               primary

(parted) ^C                                                               

Information: You may need to update /etc/fstab.

root@mint:/# mkswap /dev/sdd3                                            
Setting up swapspace version 1, size = 489 MiB (512749568 bytes)
no label, UUID=6f40ef24-7f72-47c4-b677-bc0f4a8cb3b9
root@mint:/# mkfs.ext4 -O ^64bit,^metadata_csum /dev/sdd4
mke2fs 1.47.0 (5-Feb-2023)
/dev/sdd4 contains a ext4 file system
	last mounted on /media/mint/8df37f90-aac5-4b6b-b29d-d6df08ffe73e on Wed Aug  5 14:42:00 2026
Proceed anyway? (y,N) y
Creating filesystem with 487200000 4k blocks and 121806848 inodes
Filesystem UUID: 49fd74b2-6c44-4132-832d-ae033e16c425
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968, 
	102400000, 214990848

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done       

root@mint:/# mkdir -p /tmp/wd_jessie
root@mint:/# cd /tmp/wd_jessie
root@mint:/tmp/wd_jessie# wget https://github.com/abskmj/wd-mycloud-gen1/releases/download/packages/clean-debian-jessie.tgz
--2026-08-05 15:06:26--  https://github.com/abskmj/wd-mycloud-gen1/releases/download/packages/clean-debian-jessie.tgz
Resolving github.com (github.com)... 140.82.121.4
Connecting to github.com (github.com)|140.82.121.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://release-assets.githubusercontent.com/github-production-release-asset/255866249/cf81d300-7f29-11ea-89f7-9c22f6408c20?sp=r&sv=2018-11-09&sr=b&spr=https&se=2026-08-05T15%3A55%3A08Z&rscd=attachment%3B+filename%3Dclean-debian-jessie.tgz&rsct=application%2Foctet-stream&skoid=96c2d410-5711-43a1-aedd-ab1947aa7ab0&sktid=398a6654-997b-47e9-b12b-9515b896b4de&skt=2026-08-05T14%3A55%3A08Z&ske=2026-08-05T15%3A55%3A08Z&sks=b&skv=2018-11-09&sig=huJHazkyuXRx91V9gqxmgCefh7Xh0qCdwPZtqhAFePg%3D&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmVsZWFzZS1hc3NldHMuZ2l0aHVidXNlcmNvbnRlbnQuY29tIiwia2V5Ijoia2V5MSIsImV4cCI6MTc4NTk0NTk4NywibmJmIjoxNzg1OTQyMzg3LCJwYXRoIjoicmVsZWFzZWFzc2V0cHJvZHVjdGlvbi5ibG9iLmNvcmUud2luZG93cy5uZXQifQ.Svizw6_Hcrvp3s7t5BOegD_md8A7LKUgbbkJ6obnjSY&response-content-disposition=attachment%3B%20filename%3Dclean-debian-jessie.tgz&response-content-type=application%2Foctet-stream [following]
--2026-08-05 15:06:26--  https://release-assets.githubusercontent.com/github-production-release-asset/255866249/cf81d300-7f29-11ea-89f7-9c22f6408c20?sp=r&sv=2018-11-09&sr=b&spr=https&se=2026-08-05T15%3A55%3A08Z&rscd=attachment%3B+filename%3Dclean-debian-jessie.tgz&rsct=application%2Foctet-stream&skoid=96c2d410-5711-43a1-aedd-ab1947aa7ab0&sktid=398a6654-997b-47e9-b12b-9515b896b4de&skt=2026-08-05T14%3A55%3A08Z&ske=2026-08-05T15%3A55%3A08Z&sks=b&skv=2018-11-09&sig=huJHazkyuXRx91V9gqxmgCefh7Xh0qCdwPZtqhAFePg%3D&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmVsZWFzZS1hc3NldHMuZ2l0aHVidXNlcmNvbnRlbnQuY29tIiwia2V5Ijoia2V5MSIsImV4cCI6MTc4NTk0NTk4NywibmJmIjoxNzg1OTQyMzg3LCJwYXRoIjoicmVsZWFzZWFzc2V0cHJvZHVjdGlvbi5ibG9iLmNvcmUud2luZG93cy5uZXQifQ.Svizw6_Hcrvp3s7t5BOegD_md8A7LKUgbbkJ6obnjSY&response-content-disposition=attachment%3B%20filename%3Dclean-debian-jessie.tgz&response-content-type=application%2Foctet-stream
Resolving release-assets.githubusercontent.com (release-assets.githubusercontent.com)... 185.199.109.133, 185.199.108.133, 185.199.111.133, ...
Connecting to release-assets.githubusercontent.com (release-assets.githubusercontent.com)|185.199.109.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 521275033 (497M) [application/octet-stream]
Saving to: ‘clean-debian-jessie.tgz’

clean-debian-jessie 100%[===================>] 497.13M  9.76MB/s    in 52s     

2026-08-05 15:07:18 (9.63 MB/s) - ‘clean-debian-jessie.tgz’ saved [521275033/521275033]

root@mint:/tmp/wd_jessie# tar zxvf clean-debian-jessie.tgz
config.img
kernel.img
rootfs.img
rootfs.md5
rootfs.txt
root@mint:/tmp/wd_jessie# dd if=kernel.img of=/dev/sdb5 bs=1M
3+1 records in
3+1 records out
3390408 bytes (3.4 MB, 3.2 MiB) copied, 0.00394559 s, 859 MB/s
root@mint:/tmp/wd_jessie# dd if=kernel.img of=/dev/sdd5 bs=1M
3+1 records in
3+1 records out
3390408 bytes (3.4 MB, 3.2 MiB) copied, 0.103833 s, 32.7 MB/s
root@mint:/tmp/wd_jessie# lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0   2.5G  1 loop /rofs
sda           8:0    1  29.8G  0 disk 
└─sda1        8:1    1  29.8G  0 part /cdrom
sdd           8:48   0   1.8T  0 disk 
├─sdd1        8:49   0   1.9G  0 part 
├─sdd2        8:50   0   1.9G  0 part 
├─sdd3        8:51   0   489M  0 part 
├─sdd4        8:52   0   1.8T  0 part 
├─sdd5        8:53   0    95M  0 part 
├─sdd6        8:54   0    96M  0 part 
├─sdd7        8:55   0     1M  0 part 
└─sdd8        8:56   0     2M  0 part 
nvme0n1     259:0    0 476.9G  0 disk 
├─nvme0n1p1 259:1    0   260M  0 part 
├─nvme0n1p2 259:2    0    16M  0 part 
├─nvme0n1p3 259:3    0 474.7G  0 part 
└─nvme0n1p4 259:4    0     2G  0 part 
root@mint:/tmp/wd_jessie# dd if=kernel.img of=/dev/sdd5 bs=1M
3+1 records in
3+1 records out
3390408 bytes (3.4 MB, 3.2 MiB) copied, 0.107379 s, 31.6 MB/s
root@mint:/tmp/wd_jessie# dd if=kernel.img of=/dev/sdd6 bs=1M
3+1 records in
3+1 records out
3390408 bytes (3.4 MB, 3.2 MiB) copied, 0.108887 s, 31.1 MB/s
root@mint:/tmp/wd_jessie# dd if=config.img of=/dev/sdd7 bs=1M
0+1 records in
0+1 records out
568 bytes copied, 0.00128036 s, 444 kB/s
root@mint:/tmp/wd_jessie# dd if=config.img of=/dev/sdd8 bs=1M
0+1 records in
0+1 records out
568 bytes copied, 0.00158452 s, 358 kB/s
root@mint:/tmp/wd_jessie# dd if=rootfs.img of=/dev/sdd1 bs=1M
1952+0 records in
1952+0 records out
2046820352 bytes (2.0 GB, 1.9 GiB) copied, 70.2582 s, 29.1 MB/s
root@mint:/tmp/wd_jessie# dd if=rootfs.img of=/dev/sdd2 bs=1M
1952+0 records in
1952+0 records out
2046820352 bytes (2.0 GB, 1.9 GiB) copied, 79.1821 s, 25.8 MB/s
root@mint:/tmp/wd_jessie# mdadm --stop /dev/md*
mdadm: stopped /dev/md0
root@mint:/tmp/wd_jessie# mdadm --create /dev/md0 --level=1 --metadata=0.9 --assume-clean --run --force --raid-devices=2 /dev/sdd1 /dev/sdd2
mdadm: /dev/sdd1 appears to contain an ext2fs file system
       size=1998848K  mtime=Sat Nov 28 15:32:39 2015
mdadm: /dev/sdd1 appears to be part of a raid array:
       level=raid1 devices=2 ctime=Wed Aug  5 11:26:40 2026
mdadm: /dev/sdd2 appears to contain an ext2fs file system
       size=1998848K  mtime=Sat Nov 28 15:32:39 2015
mdadm: /dev/sdd2 appears to be part of a raid array:
       level=raid1 devices=2 ctime=Wed Aug  5 11:26:40 2026
mdadm: array /dev/md0 started.
root@mint:/tmp/wd_jessie# mkdir -p /mnt/wd_root
mkdir: cannot create directory ‘/mnt/wd_root’: Input/output error
root@mint:/tmp/wd_jessie# sudo mkdir -p /mnt/wd_root
mkdir: cannot create directory ‘/mnt/wd_root’: Input/output error
root@mint:/tmp/wd_jessie# mdadm --stop /dev/md0
mdadm: stopped /dev/md0
root@mint:/tmp/wd_jessie# mdadm --create /dev/md0 --level=1 --metadata=0.9 --assume-clean --run --force --raid-devices=2 /dev/sdd1 /dev/sdd2
mdadm: /dev/sdd1 appears to contain an ext2fs file system
       size=1998848K  mtime=Sat Nov 28 15:32:39 2015
mdadm: /dev/sdd1 appears to be part of a raid array:
       level=raid1 devices=2 ctime=Wed Aug  5 15:15:39 2026
mdadm: /dev/sdd2 appears to contain an ext2fs file system
       size=1998848K  mtime=Sat Nov 28 15:32:39 2015
mdadm: /dev/sdd2 appears to be part of a raid array:
       level=raid1 devices=2 ctime=Wed Aug  5 15:15:39 2026
mdadm: array /dev/md0 started.
root@mint:/tmp/wd_jessie# dd if=kernel.img of=/dev/sdd5 bs=1M
3+1 records in
3+1 records out
3390408 bytes (3.4 MB, 3.2 MiB) copied, 0.14317 s, 23.7 MB/s
root@mint:/tmp/wd_jessie# dd if=kernel.img of=/dev/sdd6 bs=1M
3+1 records in
3+1 records out
3390408 bytes (3.4 MB, 3.2 MiB) copied, 0.157279 s, 21.6 MB/s
root@mint:/tmp/wd_jessie# dd if=config.img of=/dev/sdd7 bs=1M
0+1 records in
0+1 records out
568 bytes copied, 0.00107849 s, 527 kB/s
root@mint:/tmp/wd_jessie# dd if=config.img of=/dev/sdd8 bs=1M
0+1 records in
0+1 records out
568 bytes copied, 0.00106602 s, 533 kB/s
root@mint:/tmp/wd_jessie# dd if=rootfs.img of=/dev/sdd1 bs=1M
1952+0 records in
1952+0 records out
2046820352 bytes (2.0 GB, 1.9 GiB) copied, 5.07629 s, 403 MB/s
root@mint:/tmp/wd_jessie# dd if=rootfs.img of=/dev/sdd2 bs=1M
1952+0 records in
1952+0 records out
2046820352 bytes (2.0 GB, 1.9 GiB) copied, 53.1465 s, 38.5 MB/s
root@mint:/tmp/wd_jessie# mdadm --stop /dev/md*
mdadm: stopped /dev/md0
root@mint:/tmp/wd_jessie# mdadm --create /dev/md0 --level=1 --metadata=0.9 --assume-clean --run --force --raid-devices=2 /dev/sdd1 /dev/sdd2
mdadm: /dev/sdd1 appears to contain an ext2fs file system
       size=1998848K  mtime=Sat Nov 28 15:32:39 2015
mdadm: /dev/sdd1 appears to be part of a raid array:
       level=raid1 devices=2 ctime=Wed Aug  5 15:23:17 2026
mdadm: /dev/sdd2 appears to contain an ext2fs file system
       size=1998848K  mtime=Sat Nov 28 15:32:39 2015
mdadm: /dev/sdd2 appears to be part of a raid array:
       level=raid1 devices=2 ctime=Wed Aug  5 15:23:17 2026
mdadm: array /dev/md0 started.
root@mint:/tmp/wd_jessie# mdadm --stop /dev/md0
mdadm: stopped /dev/md0
root@mint:/tmp/wd_jessie# sync
root@mint:/tmp/wd_jessie# 
