Lecture 4 - Files and IO (Programmers POV)

- Recall synchronization between threads:
	- mutual exclusion: one thread doing one thing at a particular time (operating on the critical section via locks)
	- Lock.Acquire() and Lock.Release()
	- other tools for cooperation like semaphores
	- Semaphore (mutex):
		- non-negative integer and has operations down() and up()
			- rule (can't decrement semaphore to below 0, init value of semaphore is 1)
			- down(): waits for semaphore to become positive then decrements it by 1 (can think of waiting as a while loop + no-op)
			- up(): increments semaphore by 1, waking up a waiting down() if any
		- mutual exclusion (like lock):
	- Semaphore (signaling other threads e.g. ThreadJoin)
		- if the init value of semaphore is 0, you can "chain" together different threads
- Recall forking as a means to create new/duplicate processes
	- ![[Pasted image 20240914194752.png]]
	- forking in a multithreaded process is sus
	* forking as a means for starting a new program in shell/command line:
		* ![[Pasted image 20240914195425.png]]
		* wait - waits for a child process to finish
			* this one takes a pointer to an integer