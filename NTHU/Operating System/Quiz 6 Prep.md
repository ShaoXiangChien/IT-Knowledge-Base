![[Pasted image 20221219083933.png]] 
- c) is cylinder
- the motion of j) is seek
- h) is arm assembly
- positioning time refers to the time of both i) motion and j) motion for the head to be on the target position
- the issue with hard disk drives that is not an issue with NAND flash storage is seek time and rotational latency
- FIFO is a disk scheduling algorithm that is better for SSD than for HDD
- SSTF may have starvation
- the difference between SCAN and LOOK disk scheduling algorithms
	- SCAN moves to both extremes of the disk cylinders, while LOOK moves only to the highest or lowest cylinders that are actually requested.
- **Mirroring as in RAID-1** improves the mean time to data loss over a single hard drive
- abcude
- **Monitor with HDMI interface** is attached directly to the PCIe bus directly in a PC.
- A SCSI device connect to PCIe via a **host-bus adapter**
- the processor can know if a device is busy by reading the device's status register
- the processor would access the device registers that reside **on the hard drive's controller** to control a SATA hard drive.
- Interrupt chaining is that Multiple devices share an IRQ line, and the ISR queries each device.
- DMA
	- the DMA controller must steal cycles from the processor when it is not accessing memory , but this may cause the processor to wait for memory longer.
	- to setup, the device driver tells the device controller to transfer data to memory at starting address and the number of bytes.
	- upon completion, DMA controller interrupts the processor.
	- after setup, the device driver tells the DMA controller to start DMA transfer.
> False! The DMA controller must transfer not only to memory but also update the processor's cache or else the processor will access the wrong data.

- Unix I/O calls - `seek()` is synchronous
- **vectored I/O** have one system call perform multiple I/O operations to save overhead.
- the **file name** of a file stored **in the directory structure on disk**.
- `ioctl()` is used to issue device-specific commands.