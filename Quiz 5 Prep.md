- the logical address and the physical address have the same bindings of compile-time binding and load-time binding
- load-time binding schemes
	- the instructions issue the physical address instead of the logical address
	- the compiler must generate relocatable code
	- the base address register's value is given at load time
	- the loader substitutes the relocatable address with the base address plus the offset
	> The physical address is **not** calculated by adding the base address to the logical address on every memory access
- a problem with the combination of dynamic loading and static linking
	it could load multiple copies of the same code into memory
- fragmentation in variable, fixed-sized contiguous memory allocation
	In variable-sized allocation, all fragmentations are external.
- **compaction** requires **execution-time** address binding
- **paging** avoids external fragmentation but still can have internal fragmentation
- **Demand paging** does **not** provide better protection from other users
- **valid/invalid bit** in a demand paging system
	'i' could mean either **the page is within the process's address but nonresident** or and invalid reference.
- difference in **how a page fault is handled** compared to a regular interrupt
	The instruction that caused the page fault **must be restarted** after fault handling
- a **page replacement algorithm** decides which **frame** to replace.
- Belady's anomaly
	adding more frames could actually cause more page faults
- Enhanced Second-Chance page replacement algorithm
	a page with (0, 0) is the best page to replace because it's neither recently used nor modified
