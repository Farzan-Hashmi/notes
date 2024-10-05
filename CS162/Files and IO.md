Files and IO (Programmers POV)

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
- Purpose of the buffer in a FILE *: user level buffer
- Purpose of a lock in the FILE *: in case multiple threads are using FILE concurrently
- Purpose of the fflush method (higher level FILE API): it is meant to forcefully flush the contents of the buffer at the user level to the kernel
- Code example:
  ```
  char x = ‘c’;
  FILE* f1 = fopen(“file.txt”, “wb”);
  fwrite(“b”, sizeof(char), 1, f1);
  fflush(f1);
  FILE* f2 = fopen(“file.txt”, “rb”);
  fread(&x, sizeof(char), 1, f2);
  ```
- Mapping the kernel maintains: a mapping from descriptor to open file description
- Contents of an Open File Description (that the kernel maintains): where to find file data on disk, current position in file, etc.
- When you fork a process, both the parent and child process (in the kernel space) have the file descriptor point to the same file descriptor.
- Open File Description is Aliased:
  - Process 1
    - Thread's Regs
    - Address Space (Memory)
    - File Descriptors: 3
  - Process 2
    - Thread's Regs
    - Address Space (Memory)
    - File Descriptors: 3
  - Kernel Space: Initially contains 0, 1, and 2 (stdin, stdout, stderr)
  - User Space: Open File Description:
    - File: foo.txt
    - Position: 100
- What fork() returns for a child: 0
- What fork() returns for a parent: Greater than 0
- What int dup (int old fd) does: create a duplicate file descriptor from old fd
- What int dup2(old fd, new fd) does: custom, create a copy of old fd and assign to new fd




