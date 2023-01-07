# mdadm

# Install
sudo apt install mdadm

# OS partition info
sudo cat /proc/partitions

# Format to Linux Raid
sudo fdisk /dev/sdX
	-m menu, -l list, -d delete_part, -n new_part, -t change_part_type
	
# OS reindex partitions
sudo partprobe

# Create Raid
sudo mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

# Get creation progress
sudo watch -n1 sudo cat /proc/mdstat

# Info raid (root user)
sudo mdadm --detail --scan --verbose >> /etc/mdadm.conf

# Add disk
sudo mdadm --add /dev/md0 /dev/sde1
