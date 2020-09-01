# Disks and Filesystems

Linux Hierarchy is something like this:

- Disk
- Partition Table
    - Partition 1
    - Partition 2
        - Filesystem Data Structures
        - File Data

## Partitions

- Partitions are subdivisions of the whole disk. On Linux, they’re denoted with a number after the whole block device, and therefore have device names such as /dev/sda1 and /dev/sdb3. 

- The kernel presents each partition as a block device, just as it would an entire disk. Partitions are defined on a small area of the 
disk called a partition table.

- **(MBR)** Master Boot Record 
- **(GPT)** Globally Unique Identifier Partition Table 
- There is a critical difference between partitioning and filesystem manipulation. The partition table defines simple boundaries on the disk, whereas a filesystem is a much more involved data system.
```bash
# viewing partition table
parted -l
# or use fdisk
fdisk -l
```

### Changing Partition Tables

Altering partition tables is also relatively easy, but there are risks involved in making this kind of change to the disk. Keep the following in mind:
- Changing the partition table makes it quite difficult to recover any data on partitions that you delete because it changes the initial point of reference for a filesystem. Make sure that you have a backup if the disk you’re partitioning contains critical data.
- Ensure that no partitions on your target disk are currently in use. This is a concern because most Linux distributions automatically mount any detected filesystem. (See Section 4.2.3 for more on mounting and unmounting.)

#### **Difference between parted and fdisk**

That said, there is a major difference in the way that fdisk and parted work. With fdisk, you design your new partition table before making the actual changes to the disk; fdisk only makes the changes as you exit the program. But with parted, partitions are created, modified, and removed as you issue the commands. You don’t get the chance to review the partition table
before you change it.

- **fdisk** After modifying the partition table, fdisk issues a single system call on the disk to tell the kernel that it should reread the partition table.

- **parted** In comparison, the parted tools do not use this disk-wide system call. Instead, they signal the kernel when individual partitions are altered. After processing a single partition change, the kernel does not produce the preceding debugging output.

```bash
# see the outputs use command
dmesg
```
-----
## Filesystems
- The last link between the kernel and user space for disks is typically the filesystem; this is what you’re accustomed to interacting with when you run commands such as ls and cd.

- The filesystem is a form of database; it supplies the structure to transform a simple block device into the sophisticated hierarchy of files and subdirectories that users can understand.

- Filesystems are also traditionally implemented in the kernel, but the innovation of **Plan 9** has inspired the development of user-space filesystems. The **File System in User Space (FUSE)** feature allows user-space filesystems in Linux.

- The **Virtual File System (VFS)** abstraction layer completes the filesystem implementation. Much as the SCSI subsystem standardizes communication between different device types and kernel control commands, VFS ensures that all filesystem implementations support a standard interface so that user-space applications access files and directories in the same manner.

### Creating a Filesystem

- Creating a filesystem comes after creation of partitions.

```bash
# you can create an ext4 partition on /dev/sdf2 with this command
mkfs -t ext4 /dev/sdf2
```

- The mkfs program automatically determines the number of blocks in a device and sets some reasonable defaults.

- When you create a filesystem, mkfs prints diagnostic output as it works, including output pertaining to the superblock. The superblock is a key component at the top level of the filesystem database

- when you run `mkfs -t ext4`, mkfs in turn runs `mkfs.ext4`. You can see all mkfs type funcitons with `ls -l /sbin/mkfs.*`. 

### **Mounting a Filesystem**

- On Unix, the process of attaching a filesystem is called mounting. When the system boots, the kernel reads some configuration data and mounts root (/) based on the configuration data.

- When mounting a filesystem, the common terminology is “mount a
device on a mount point.”

To learn the current filesystem status of your system, run `mount`.

To mount a filesystem you can use `mount -t type device mountpoint`, eg.
```bash
# You normally don’t need to supply the -t type option because mount can usually figure it out for you.
mount -t ext4 /dev/sdf2 /home/extra

# to unmount use the command umount
umount mountpoint
```

#### **Mounting options**

- **-r**
    
    The -r option mounts the filesystem in read-only mode. This has a number of uses, from write protection to bootstrapping.
- **-n** 
    
    The -n option ensures that mount does not try to update the system runtime mount database, /etc/mtab. The mount operation fails when it cannot write to this file, which is important at boot time because the root partition (and, therefore, the system mount database) is read-only at first.
- **-t** 

    The -t type option specifies the filesystem type.


#### **Remounting**
- There will be times when you may need to reattach a currently mounted filesystem at the same mount point when you need to change mount options.
- The most common such situation is when you need to make a read-only filesystem writable during crash recovery.
```bash
# This command assumes that the correct device listing for / is in
#/etc/fstab (as discussed in the next section). If it is not, you 
# must specify the device.
mount -n -o remount /
```

## Filesystem Capacity
To view the size and utilization of your currently mounted filesystems, use the `df` command.
```bash
# get the capacity of filesystem
df
# human readable format
df -h
# show in MBs
df -m 
```

## Checking and Repairing Filesystems
- The optimizations that Unix filesystems offer are made possible by a sophisticated database mechanism. For filesystems to work seamlessly, the kernel has to trust that there are no errors in a mounted filesystem. If errors exist, data loss and system crashes may result.

- Filesystem errors are usually due to a user shutting down the system in a rude way (for example, by pulling out the power cord). In such cases, the filesystem cache in memory may not match the data on the disk, and the system also may be in the process of altering the filesystem when you happen to give the computer a kick.

- Although a new generation of filesystems supports journals to make filesystem corruption far less common, you should always shut the system down properly. And regardless of the filesystem in use, filesystem checks are still necessary every now and to maintain sanity

- The tool to check a filesystem is `fsck`.

- **You should never use fsck on a mounted filesystem** because the kernel may alter the disk data as you run the check, causing runtime mismatches that can crash your system and corrupt files.
```bash
# manual mode, asks questions,
# argument is a mountpoint as listed in /etc/fstab
fsck mountpoint
# run automatic mode
fsck -p mountpoint
# non modifier mode
fsck -n mountpoint
```

- If fsck finds a problem in manual mode, it stops and asks you a question relevant to fixing the problem. These questions deal with the internal structure of the filesystem, such as reconnecting loose inodes and clearing blocks (an inode is a building block of the filesystem). When fsck asks you about reconnecting an inode, __it has found a file that doesn’t appear to have a name__. 

- When reconnecting such a file, fsck places the file in the lost+found directory in the filesystem, with a number as the filename. If this happens, you need to guess the name
based on the content of the file; the original name is probably gone.

- In general, it’s pointless to sit through the fsck repair process if you’ve just uncleanly shut down the system, because fsck may have a lot of minor errors to fix. 

- Fortunately, e2fsck has a `-p` option that automatically fixes
ordinary problems without asking and aborts when there’s a serious error.
In fact, Linux distributions run some variant of `fsck -p` at boot time.

- If you think that something really bad has happened, try running
`fsck -n` to **check the filesystem without modifying anything**. 


## Special-Purpose Filesystems

- Not all filesystems represent storage on physical media. Specifically, most versions of Unix have filesystems that serve as system interfaces. That is, rather than serving only as a means to store data on a device, a filesystem can represent system information such as process IDs and kernel diagnostics. 

- This idea goes back to the /dev mechanism, which is an early model of using files for I/O interfaces. The /proc idea came from the eighth edition of research Unix, implemented by Tom J. Killian and accelerated when Bell Labs (including many of the original Unix designers) created Plan 9—a research operating system that took filesystem abstraction to a whole new level.

### **proc** 
- proc Mounted on /proc. The name proc is actually an **abbreviation for process**. Each numbered directory inside /proc is actually the process ID of a current process on the system; the files in those directories represent various aspects of the processes. 

- The file /proc/self represents the current process. The Linux proc filesystem includes a great deal of additional kernel and hardware information in files like /proc/cpuinfo. (There has been a push to move information unrelated to processes out of /proc and into /sys.)


## Swap Space
- Not every partition on a disk contains a filesystem. It’s also possible to augment the RAM on a machine with disk space. If you run out of real memory, the Linux virtual memory system can automatically move pieces of memory to and from a disk storage. 

- This is called swapping because pieces of idle programs are swapped to the disk in exchange for active pieces residing on the disk. The disk area used to store memory pages is called swap space (or just swap for short).

```bash
# to see the swap space use free command
free
# human readable format
free -h
```

### **Using a Disk Partition as Swap Space**
1. Make sure the partition is empty.
2. Run mkswap dev, where dev is the partition’s device. This command puts
a swap signature on the partition.
3. Execute swapon dev to register the space with the kernel.
4. After creating a swap partition, you can put a new swap entry in your `/etc/fstab` file to make the system use the swap space as soon as the machine boots.

### **Using a File as Swap Space**

```bash
# num_mb = file size in MB
dd if=/dev/zero of=swap_file bs=1024k count=num_mb
mkswap swap_file
swapon swap_file
# to remove the swap partition or file from kernels active swap pool
swapoff swap_file
# or use a mountpoint
swapoff mountpoint
```
