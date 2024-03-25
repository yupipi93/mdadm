# Steps to create RAID whith MDADM

## Install
```sh
sudo apt install mdadm
```
## OS partition info
```
sudo cat /proc/partitions
```
## Format to Linux Raid
Format all disk except OS disk to Linux raid.
1. format and change format to raid.
```
sudo fdisk /dev/sdX
```
-m menu, -l list, -d delete_part, -n new_part, -t change_part_type
	
## OS reindex partitions
```
sudo partprobe
```
## Create Raid
```
sudo mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
```

## Get creation progress
```
sudo watch -n1 sudo cat /proc/mdstat
```

## Get all raids info (root user)
```
sudo mdadm --detail --scan --verbose >> /etc/mdadm.conf
sudo mdadm --examine /dev/sdXY
```

## Add disk
```
sudo mdadm --add /dev/md0 /dev/sde1
```

## Check raid info
```
sudo mdadm -D /dev/md0
```

## Resize to max
```
sudo mdadm --grow /dev/md0 -z max
```

## Resize
```
sudo resize2fs /dev/md0
```

## Init check status process(Bussy disk for a aboud 3 hours)
```
sudo /usr/share/mdadm/checkarray -a /dev/md0
```

## Stop check status
```
echo idle | sudo tee /sys/block/md0/md/sync_action > /dev/null
```




## Current menesteres config
```
/dev/md0:
           Version : 1.2
     Creation Time : Sun Jan  1 21:02:26 2023
        Raid Level : raid6
        Array Size : 2929889280 (2794.16 GiB 3000.21 GB)
     Used Dev Size : 976629760 (931.39 GiB 1000.07 GB)
      Raid Devices : 5
     Total Devices : 6
       Persistence : Superblock is persistent

     Intent Bitmap : Internal

       Update Time : Wed Nov  1 18:10:01 2023
             State : active
    Active Devices : 5
   Working Devices : 6
    Failed Devices : 0
     Spare Devices : 1

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : bitmap

              Name : quantumpc:0  (local to host quantumpc)
              UUID : f0db9b49:e4322e57:02eaa311:2dc9bad9
            Events : 41807

    Number   Major   Minor   RaidDevice State
       0       8       17        0      active sync   /dev/sdb1
       5       8       97        1      active sync   /dev/sdg1
       2       8       49        2      active sync   /dev/sdd1
       3       8       81        3      active sync   /dev/sdf1
       6       8       32        4      active sync   /dev/sdc

```

       7       8       65        -      spare   /dev/sde1

## Check status
sudo mdadm --detail /dev/md0
sudo watch -n1 sudo cat /proc/mdstat (check continuously)
