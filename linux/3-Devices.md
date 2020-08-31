## Device Files

- It is easy to manipulate most devices on a Unix system because the kernel presents many of the device I/O interfaces to user processes as files. These device files are sometimes called __device nodes__. Not only can a programmer use regular file operations to work with a device, but some devices are also accessible to standard programs like cat, so you don’t have to be a programmer to use a device. 
- However, there is a limit to what you can do with a file interface, so __not all devices or device capabilities are accessible with standard file I/O__.

- Device files are in the /dev directory, and running `ls /dev` reveals more than a few files in /dev. 

### File modes of devices:
- **b - Block Device** 
    - Programs access data from a block device in fixed chunks. Disk devices are type of block device. 
    - Disks can be easily split up into blocks of data. Because a block device’s total size is fixed and easy to index, processes have random access to any block in the device with the help of the kernel
- **c - Character Device** 
    - Character devices work with data streams. You can only read characters from or write characters to character devices, such as /dev/null. 
    - Character devices don’t have a size; when you read from or write to one, the kernel usually performs a read or write operation on the device. Printers directly attached to your computer are represented by character devices. 
    - It’s important to note that during character device interaction, the kernel cannot back up and reexamine the data stream after it has passed data to a device or process.
- **p - Pipe Device** 
    - Named pipes are like character devices, with another process at the other end of the I/O stream instead of a kernel driver.
- **s - Socket Device** 
    - Sockets are special-purpose interfaces that are frequently used for interprocess communication. They’re often found outside of the /dev directory. Socket files represent Unix domain sockets


- Not all devices have device files because the block and character device I/O interfaces are not appropriate in all cases. For example, network interfaces don’t have device files. It is theoretically possible to interact with a network interface using a single character device, but because it would be exceptionally difficult, the kernel uses other I/O interfaces.

#### dd and Devices
- The program dd is extremely useful when working with block and character devices. This program’s sole function is to read from an input file or stream and write to an output file or stream, possibly doing some encoding conversion on the way.

- dd copies data in blocks of a fixed size. Here’s how to use dd with a character device and some common options:
    ```bash
    # syntax is based on an old IBM Job Control Language(JCL) style.
    # if - input file
    # of - output file
    # bs - block size
    # count - total num of block size to copy
    # skip - num of blocks to skip ahead 
    dd if=/dev/zero of=new_file bs=1024 count=1 skip=0
    ```
