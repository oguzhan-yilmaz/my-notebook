# The Big Picture
Processes —the running programs that the kernel manages— collectively make up the system’s upper level, called user space.


The kernel runs in kernel mode, and the user processes run in user mode. The area that only the kernel can access is called kernel space.  


User mode, in comparison, restricts access to a (usually quite small) subset of memory and safe CPU operations. User space refers to the parts of main memory that the user processes can access


### The Kernel

The kernel is in charge of managing tasks in four general system areas:

**Processes** The kernel is responsible for determining which processes
are allowed to use the CPU.

**Memory** The kernel needs to keep track of all memory—what is currently allocated to a particular process, what might be shared between processes, and what is free.

**Device drivers** The kernel acts as an interface between hardware (such as a disk) and processes. It’s usually the kernel’s job to operate the hardware.

**System calls and support** Processes normally use system calls to communicate with the kernel.

-----

- Other than init, all user processes on a Linux system start as a result of **fork()**, and most of the time, you also run **exec()** to start a new program instead of running a copy of an existing process.

### Users
The Linux kernel supports the traditional concept of a Unix user. A user is an entity that can __run processes and own files__. A user is associated with a username. However, the kernel does not manage the usernames; instead, it identifies users by simple numeric identifiers called **userids**.

