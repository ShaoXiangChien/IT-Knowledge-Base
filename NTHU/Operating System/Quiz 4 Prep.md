## Important Points
- Four cases when *a preemtive CPU scheduler can take control*
	1. upon process termination
	2. upon I/O completion
	3. upon starting an I/O
	4. upon a timer interrupt
	==not upon making a system call==
- Conceptual difference between a *dispatcher* and a *scheduler*
	The *dispatcher* switches to the process chosen by the *scheduler*
- scheduling criteria
	*turnaround time* is the amount of time from *job submission* to *completion*
- shortest-job-first (SJF) scheduling - selects the job with the shortest *next CPU burst*
- first-come-first-serve (FCFS) scheduling - shorter jobs can suffer from the *convoy effect*
- round-robin (RR) scheduling - for *short* time quantum, RR becomes similar to *FCFS*
- *aging* is one way of *preventing starvation*
- multiprocessor scheduling
	*asymmetric multiprocessing* means multiple slave processors execute the schedule computed by one single master processor.
- rate monotonic (RM) scheduling, assuming no resource sharing
	the priority depends on only the *period* but not *the execution time of the task*
- earliest deadline first (EDF) vs. rate monotonic (RM) scheduling
	**RM** tasks have *static priority* whereas **EDF** tasks have *dynamic priority* 
- requirement of a *critical section* (CS)
	- there exists a limit on the number of times that other processes are allowed to enter their CS after a process has made its request to enter its CS.
	- a *bound* must exist on the number of times that other processes are allowed to enter their CS after the process has made a request to enter its CS
	- at most one process is executing in its CS at any same time
	- when no proess is in its CS, a process wishing to enter its CS must not block indefinitely
	==false! after a process has requested to enter its CS, other processes are allowed to enter their CS **for a finite number of times**==
- `wait(S)` and `signal(S)`, where S is semaphore - `wait(S)` may block, but `signal(S)` never blocks
- 17. semaphore 1, 0, n
- 18. empty, mutex, mutex, full
- *rw_mutex* allows *at most one* writer to block out *all other* process or the *first reader* to block *all writer*
- 20. `wait(b); S2;`