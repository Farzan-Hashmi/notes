Threads and Processes
- Recall: Illusion of Multiple Processors
	- threads: virtual cores
	- multiple threads: multiplex hardware in time
	- executed on processor when it is resident in the processor's registers
	- maintains its own PC, stack pointer, etc.
- Recall: (Virtual) Address Space
	- set of accessible addresses + state associated with them
	- virtual address space: processors view of the memory
		- independent of physical storage
	- ![[Pasted image 20240912212844.png]]
- Recall: Process
	- protected environment with 1+ threads and a protected address space
	- instance of a running program
- Recall: Dual Mode Operation:
	- processes execute in user mode, kernels execute in kernel mode
	- carefully controlled transitions between user mode and kernel mode (system calls, interrupts, exceptions)
- Thread definitions:
	- single unique execution context
	- abstraction of execution sequence that represents a separately schedulable task
	- mechanism of concurrency (overlapping execution) but can also run in parallel (simultaneous execution)
- Motivation for Threads:
	- OS's need to handle multiple things at once (mouse, i/o, MTAO)
		- network servers, parallel programs, user interface (mouse, mic, camera)
	- threads can represent one thing or task
- multiprocessing vs multiprogramming:
	- multiprocessing = multiple cores/cpus on the same task
	- multiprogramming = multiple jobs/processes (not necessarily simultaneously)
	- multithreading = multiple threads/processes
	- two threads running concurrently: scheduler can run two threads in any order/any interleaving
- concurrency vs parallelism:
	- concurrency is MTAO, parallelism is specifically simultaneous and implies concurrency
- threads are in one of the following three states/queues:
	- running
	- ready (eligible)
	- blocked (maybe due to some i/o operation it is waiting for)
- once process starts, it issues system calls to create new threads
- ![[Pasted image 20240912225757.png]]
- OS lib api for threads: pthreads
- ![[Pasted image 20240913030900.png]]
- Programmer's view is like an infinite number of threads and processors. But what actually happens is that the scheduler interleaves and reorders the threads so we need to code multithreaded programs keeping this in mind
	- assume x == 0 and y == 0
		- Thread A does:
			- x = y + 1
		- Thread B does:
			- y = 2
			- y = y * 2
	- possible values of x can be 1, 3, or 5 (race condition! non deterministic)
- synchronization: coordination among threads, usually regarding shared data
	- mutual exclusion: ensures only one thread does a particular thing at a time
- critical section (code exactly one thread can execute at once -> protected by a lock)
- lock: object only on thread can hold at a time
	- Lock.acquire() wait until lock is free then mark as busy. after this returns, we say the thread holds the lock
	- Lock.release() mark lock as free
- pthreads has a mutex (lock) option
	- ![[Pasted image 20240913032146.png]]
- Processes
	- first process started by kernel
	- then other processes are created from processes
	- exit terminates a process
	- fork creates a brand new process by copying the current process entirely
		- copies everything to a different address space to create a new process
			- good example here: https://unix.stackexchange.com/a/57176

