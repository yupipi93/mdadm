# Steps to create RAID whith MDADM

## Install
sudo apt install mdadm

## OS partition info
sudo cat /proc/partitions

## Format to Linux Raid
sudo fdisk /dev/sdX
	-m menu, -l list, -d delete_part, -n new_part, -t change_part_type
	
## OS reindex partitions
sudo partprobe

## Create Raid
sudo mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

## Get creation progress
sudo watch -n1 sudo cat /proc/mdstat

## Get all raids info (root user)
sudo mdadm --detail --scan --verbose >> /etc/mdadm.conf
sudo mdadm --examine /dev/sdXY

## Add disk
sudo mdadm --add /dev/md0 /dev/sde1

## Check raid capacity
sudo mdadm -D /dev/md0 | grep -e "Array Size" -e "Dev Size"

## Resize to max
sudo mdadm --grow /dev/md0 -z max

## Resize
sudo resize2fs /dev/md0

## Init check status process(Bussy disk for a aboud 3 hours)
sudo /usr/share/mdadm/checkarray -a /dev/md0

## Stop check status
echo idle | sudo tee /sys/block/md0/md/sync_action > /dev/null

## Check status
sudo mdadm --detail /dev/md0
sudo watch -n1 sudo cat /proc/mdstat (check continuously)
