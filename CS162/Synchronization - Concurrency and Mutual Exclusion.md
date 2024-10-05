Synchronization: Concurrency and Mutual Exclusion
- Alternative to the kernel lock: atomic instruction sequences that the hardware ensures
- Test&set (&address): atomic instruction to grab whatever is at the address (return it) and also store 1 at the address
- Atomic instruction swap (&address, register): swaps value between register and memory location specified by address
- Atomic instruction compare&swap (&address, reg1, reg2): checks if memory at value address == value of reg1. if so save reg2 value at that memory location and return success. else return false
- Basic atomic instruction version of lock:
  ```c
  // (Free) Can access this memory location from user space!
  int mylock = 0; // Interface: acquire(&mylock); // release(&mylock);
  
  acquire(int *thelock) {
      while (test&set(thelock)); // Atomic operation!
  }
  
  release(int *thelock) {
      *thelock = 0; // Atomic operation!
  }
  ```
- Drawback with this implementation of lock using test&set: busy-waiting: thread consumes cycles while waiting
- Futex: an interface to the kernel sleep()
- Better Locks using test&set:
  - Can we build test&set locks without busy-waiting? Mostly. Idea: only busy-wait to atomically check lock value.
  ```c
  int guard = 0; // Global Variable!
  int mylock = FREE; // Interface: acquire(&mylock); release(&mylock);
  
  acquire(int *thelock) {
      // Short busy-wait time
      while (test&set(guard));
      if (*thelock == BUSY) {
          put thread on wait queue;
          go to sleep() & guard = 0;
          // guard = 0 on wakup!
      } else {
          *thelock = BUSY;
          guard = 0;
      }
  }
  
  release(int *thelock) {
      // Short busy-wait time
      while (test&set(guard));
      if anyone on wait queue {
          take thread off wait queue;
          Place on ready queue;
      } else {
          *thelock = FREE;
      }
      guard = 0;
  }
  ```
- Note: sleep has to be sure to reset the guard variable.
- If futex_op == FUTEX_WAIT: if the val equals the thing pointed to by uaddr, only then do we sleep!
```c
int futex(int *uaddr, int futex_op, int val, const struct timespec *timeout);
```
- In exit bridge conditions:
  - First condition checks to prioritize all waiting cars in one direction first (even if waiting cars in other direction never runs).
  - Else if checks for the case when there are no more cars in that direction waiting so we switch directions.
  ```c
  ArriveBridge(int direction) {
      lock_acquire(&cond_lock);
      while (cars_on_bridge == 3 || 
             (cars_on_bridge > 0 && curr_direction != direction)) {
          waiters[direction]++;
          cond_wait(&cond_directions[direction], &cond_lock);
          waiters[direction]--;
      }
      cars_on_bridge++;
      curr_direction = direction;
      lock_release(&cond_lock);
  }
  ```