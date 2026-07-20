The Linux operating system can be organized into three different levels of abstraction.
- Physical layer/hardware - which includes  CPU, memory, hard disks, networking ports, etc. 
- User space - which includes programs we are running
-  Kernel exists between them and it's job is to communicate with the hardware to make sure it does what we want our processes to do.

The kernel has 4 jobs:
	- Memory management: Keep track of how much memory is used to store what, and where
	- Process management: Determine which processes can use the central processing unit (CPU), when, and for how long
	- Device drivers: Act as mediator/interpreter between the hardware and processes
	- System calls and security: Receive requests for service from the processes

Why do user space and kernel exist as two separate layers? Because they both operate in different modes - the kernel operates in **kernel mode** and the user space operates in **user mode**.

In **kernel mode**, the kernel has complete access to the hardware, it controls everything. When we want to do anything that involves hardware, reading data from our disks, writing data to our disks, controlling our network, etc, it is all done in kernel mode. In **user space mode** on the other hand, there is a very small amount of safe memory and CPU that you are allowed to access.

So how are we able to write anything to our hardware? The answer is with system calls, system calls allow us to perform a privileged instruction in kernel mode and then switch back to user mode.

System calls (syscall) provide user space processes a way to request the kernel to do something for us. The kernel makes certain services available through the system call API. 

If we are running the ls program in non-privilege mode, the system sees I'm trying to make a syscall and it switches me to kernel mode where it executes what we requested and then returns us to user mode and your process will receive a return status if it was successful or if it had an error.

It is possible to install multiple kernels on the system, and we would choose which kernel to boot to through the GRUB menu. To check the kernel version that our system uses, use the:
`$ uname -r`