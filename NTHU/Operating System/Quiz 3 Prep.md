## Main points
- *process* vs *thread*
	- process - has its own address space
	- thread - share the same address space if they're in a given process
- concurrency and parallelism
	- concurrenecy - 看起來同時，實際上是overlap time period
	- parallelism - multicore / multiprocess -> 真的同時
- kernel threads - managed by OS directly but not only run in kernel mode
- recursive calls on a multiprocessor
- *scheduler activation*
- kernel thread vs hardware thread
	- kernel threads may run on a given hardward thread
- `join()` vs `detach()`
	- join - main thread stop running and wait until the new thread ends
	- detach - doesn't stop the main thread, just mark it
- cancel a thread is to ask thread to terminate itself
- Series-Parallel aka fork-join parallelism
	- serial -> fork -> join
	- e.g. MergeSort, QuickSort
- RET in 8051 knows where to return to by ==finding the return address in the top two bytes of the stack==
- 

## Terms
- overhead
- RPC - remote procedure call
- stub function
- marshal - sort, order
- PSW - program status word
- SMT architectures (simultaneous multithreading)
- 