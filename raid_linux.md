# Some experience about manipulate software RAID on linux using %mdadm%

# Step 1: Create partition the Disks for RAID

First and foremost, we have to partition the disks (/dev/sdb, /dev/sdc and /dev/sdd) before adding to a RAID, So let us define the partition using ‘fdisk’ command, before forwarding to the next steps.

# fdisk /dev/sdb
# fdisk /dev/sdc
# fdisk /dev/sdd

Create /dev/sdb Partition

Please follow the below instructions to create partition on /dev/sdb drive.

    Press ‘n‘ for creating new partition.
    Then choose ‘P‘ for Primary partition. Here we are choosing Primary because there is no partitions defined yet.
    Then choose ‘1‘ to be the first partition. By default it will be 1.
    Here for cylinder size we don’t have to choose the specified size because we need the whole partition for RAID so just Press Enter two times to choose the default full size.
    Next press ‘p‘ to print the created partition.
    Change the Type, If we need to know the every available types Press ‘L‘.
    Here, we are selecting ‘fd‘ as my type is RAID.
    Next press ‘p‘ to print the defined partition.
    Then again use ‘p‘ to print the changes what we have made.
    Use ‘w‘ to write the changes.

Same as /dev/sdc and /dev/sdd.

Now we have larger disk, fdisk can not handle of them, so we use %parted%

# parted -a optimal /dev/sdb
# parted -a optimal /dev/sdc
# parted -a optimal /dev/sdd

Please follow the below instructions to create partition on /dev/sdb drive.
	(parted) print
	(parted) mklabel gpt
	(parted) mkpart primary 0 -1
	(parted) set 1 raid on
	(parted) quit
	
Same as /dev/sdc and /dev/sdd

# Step 2: Creating md device md0

Now create a Raid device ‘md0‘ (i.e. /dev/md0) and include raid level on all newly created partitions (sdb1, sdc1 and sdd1) using below command.

# mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb1 /dev/sdc1 /dev/sdd1
or
# mdadm -C /dev/md0 -l=5 -n=3 /dev/sd[b-d]1

After creating raid device, check and verify the RAID, devices included and RAID Level from the mdstat output.

# cat /proc/mdstat

Step 3: Save Raid 5 Configuration

17. As mentioned earlier in requirement section, by default RAID don’t have a config file. We have to save it manually. If this step is not followed RAID device will not be in md0, it will be in some other random number.

So, we must have to save the configuration before system reboot. If the configuration is saved it will be loaded to the kernel during the system reboot and RAID will also gets loaded.

# mdadm --detail --scan --verbose >> /etc/mdadm.conf

# Remove RAID

# unmount /dev/md0
# mdadm --stop /dev/md0
# mdadm --remove /dev/md0
# mdadm --zero-superblock /dev/sdb1
# mdadm --zero-superblock /dev/sdc1
# mdadm --zero-superblock /dev/sdd1

If you got message like as:
# mdadm --zero-superblock /dev/sdb1 
mdadm: Cannot open /dev/sdb1: Device or resource busy

and then cat /proc/mdstat show as:

# cat /proc/mdstat
Personelities
...

do as follow:
# mv /sbin/mdadm /sbin/mdadm.moved
# /sbin/mdadm.moved --stop --scan
# mv /sbin/mdadm.moved /sbin/mdadm
# mdadm --zero-superblock /dev/sdb1

#Auto mount devices at startup

Add a line into /etc/fstab as below

path_to_device   mount_point   partition_format defaults 0 0

if path_to_device is connect via net (e.x. storage), we have to change "defaults" to "_netdev". This way the mount point will be mounted only after the network start correctly.
