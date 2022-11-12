## Definitions and Short Answers
1.  What is the **kernel** part of an OS?
    The one directly controls an OS and cannot touch by user.
2.  What is a **system program**?
    Program loaders that prepare OS to start running a program
3.  Is a **web browser** part of an OS?  part of the kernel?  part of system programs? 
    Middleware - layer crossing the network
4.  **Middleware** is a set of software frameworks that provide additional services to what kind of users?  What would be some example features?
    provide service to developers, such as databases, mutlimedia, and graphics
5.  As a resource manager, what kinds of resources does an OS manage?
    CPU time, storage space, use of I/O devices
6.  What does **API** stand for?  What kinds of API does an OS provide?
    Application Programming Interfaces, providing services like
    - use of memory
    - I/O devices
    - storage
    - communication
7.  What is a **bootloader**?  How is it related to an OS?
    CPU's initial program in firmware
    - do enough to load in the OS kernel
8.  How does a bootloader load an OS?
    copy OS image from storage into memory and jump to starting point of OS
9.  How does a CPU **send a command** to a device controller?  
    Interrupt Request (IRQ)
10.  From textbook (page 7) A **device controller** maintains some "**local buffer storage**" and a set of **special-purpose registers**".  Where do these storage and registers reside?  On CPU? in main memory?  in cache?  outside CPU?
    outside CPU, on device controller itself
11.  What is a **device driver**?  hardware or software?  part of the OS or in an application program?  What does it do?
    Typically, operating systems have a device driver for each device controller. This device driver understands the device controller and provides the rest of the operating system with a uniform interface to the device. To start an I/O operation, the device driver loads the appropriate registers within the device controller
12.  What does **IRQ** stand for, and what is its purpose?
    Interrupt Requests, the signal to device controller informing that the data is ready.
13.  How does a program for 8051 know when the UART has received a byte?
    through a special function registers Rl, == 1 if a char is received
14.  What does **ISR** stand for, and when is it invoked?
    Interrupt service routine
15.  What is an **interrupt vector**?
    
16.  What is an **interrupt vector table**?
    
17.  How does a processor decide which ISR to execute when there are multiple I/O devices?
    
18.  How does a processor continue executing the user program after an ISR finishes?
    
19.  What is a **nested interrupt**? 
    
20.  What is **interrupt chaining**? (page 10 in textbook)  Why is it useful?
    
21.  How does the OS protect CPU time as a resource by preventing a user program from hogging the CPU without making a system call?
    
22.  What is **volatile memory** vs. **nonvolatile memory**?  What are examples of each kind?
    
23.  What is the purpose of a **cache**?  Can a processor run without a cache?
    
24.  What is the principle of **locality**?  What are **two kinds of locality**?
    
25.  What does **DMA** stand for, and why is it used?  
    
26.  What are steps in a DMA setup, transfer, and completion?
    