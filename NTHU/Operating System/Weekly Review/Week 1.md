## Definitions and Short Answers
1.  What is **batch processing**?  What are its advantages? Disadvantages?
	User send job to operator, and the operator sorts jobs. The jobs are executed once reaching certain amount.
	### Advantages
	- repeated jobs are done without user interaction
	- sharing system for multiple users
	- assign specific time for batch jobs
	Disadvantages
	- one job at a time
	- CPU is often idle
2.  What is **multiprogramming**?  What disadvantage of batch processing does it address?
	Overlap I/O computation of jobs. Keeps both CPU & I/O devices utilized in parallel
	Deal with the idle CPU time of batch processing
3.  Compare multiprogramming and multitasking in terms of number of users, number of jobs running, and need for support features.
	![](assets/ch1-comparison-summary.png)
4.  What is an **instruction set architecture** (ISA)?  How is it different from a CPU or a processor?
	ISA is the syntax and commands of an assembly language. It specifies how you can control the CPU
5.  What are reasons for the trend from single processor to multiprocessor architectures?
	- energy efficiency
	- 
6.  What makes tightly coupled multiprocessors difficult to scale to a large number of processors?
	- requires extensive synchronization
7.  What are examples of real-time systems?  How do they differ from high-performance systems?
	examples:
	- multimedia system
	- industrial control
	- flight
	- auto control
	- anti-lock brake
	They guarantee timing constraints instead of running fast
8. What are examples of hard real-time vs soft real-time systems?
	- soft real-time: multimedia streaming
	- hard real-time: nuclear power plant controller