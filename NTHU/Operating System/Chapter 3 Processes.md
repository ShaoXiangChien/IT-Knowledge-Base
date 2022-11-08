## Process Concept
### Program vs Process
- Program - executable code
- Process - an instance of a program in execution
- synonym of *process* : job, task

### Diagram of Process State
![[assets/Pasted image 20221107000006.png]]
### Threads aka "lightweight processes"
- multiple threads may belong to one process
- share code section, data section, OS resources in a process
- per thread: thread ID, pc, register set, and stack

## Process Scheduling
- OS Purpose - multiprogramming, time-sharing
- Scheduling - OS decides when to run each process and for how long
- Overhead - time spent by OS, not productive for the user
- 