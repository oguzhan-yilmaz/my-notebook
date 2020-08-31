## Device Files

- It is easy to manipulate most devices on a Unix system because the kernel presents many of the device I/O interfaces to user processes as files. These device files are sometimes called __device nodes__. Not only can a programmer use regular file operations to work with a device, but some devices are also accessible to standard programs like cat, so you don’t have to be a programmer to use a device. 
- However, there is a limit to what you can do with a file interface, so __not all devices or device capabilities are accessible with standard file I/O__.

- Device files are in the /dev directory, and running ls /dev reveals more than a few files in /dev. 
