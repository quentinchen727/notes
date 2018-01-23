EOF: end of file. Its value is system-dependent(commonly -1 in C) and is distinct from character codes some thing else.

Python: bool is a subclass of int; True is a instance of bool; so we can multiple True or False with int

URI vs URL
URL is always URI, but the opposite is not true.
URL has access method in it, e.g. http://a.b.c, ftp://a.b.c
URI does not have to have access method: a.b.c/d

Understanding locks:
A thread wanting a thread which is required by other thread can spin or sleep.(busy waiting). This is called a spinlock. This uses a "test-and-set" instruction.
Spinning occurs when a thread that wants a specific lock continusouly checks that lock to see if it is still taken, intead of yielding CPU-time to another thread.
Alternatively, a thread that tries to takes a lock that is held waits for notification from the lock and goes into a sleeping state. The thread will then wait passively for the lock to be released.

"test-and-set" can be implemented in hardward and software level, and hardward level is preferred. "test-and-set" instruction is an instruction used to write 1(st) to a memory location and return its old value as a single atomic operation.

## Multiple producers and consumers: one lock and two conditionals for notFull and notEmpty
class BoundedBuffer {
    final Lock lock = new ReentranceLock();
    final Condition notFull = lock.newCondition(); //Condition variable bounded to a lock
    final Condition notEmpty = lock.newCondition();
    
    final Object[] items = new Object[10];
    int putPtr, takeptr, count;
    
    public void put(Object x) throws InterruptedException {
        lock.lock();
        try {
            while (count == item.length)
                notFull.await(); // if awakened up, still need to check count to break
                                 // out of while loop.
        items[putptr] = x;
        if (++putptr == items.length) putptr = 0; //same as putptr = (ptr+1)%items.length
        ++count;
        notEmpty.signal();
        }
        finally {
            lock.unlock();
        }
    } 

    pubilc  Object take() throws InterruptedExecption {
        lock.lock();
        try {
            while (count == 0) 
                notEmpty.await();
            Object x = items[takeptr];
            if (++takeptr == items.length) takeptr = 0;
            -- count;
            notFull.signal();
            return x;
        }
        finally {
            lock.unlock();
        }
    }
}
// Condition variable provides a light-weighted solution than simple lock protection.

## Readers-Writers problem
1. If one is writing, no one else can read or write
2. If someone is reading, others may read it at the same time, but no one can write.
Solution prioritizing readers: one writer lock, one reader count lock, one readcnt variable
writers: {
    wait(wrt); // wait on writer lock
    // perform the write
    signal(wrt); // release writer lock;
}
readers: {
    wait(mutex); // enter critical section
    readcnt++; // increase reader count
    if (readcnt == 1)
        wait(wrt); // if there is reader, blocking incoming writer, or itself 
                   // will be blocked if there is an active writer
    signal(mutex); // release lock for readcnt. signal the next reader
    // perform reading
    wait(mutex); // a reader wants to leave, it needs to update the readcnt
    readcnt--;
    if(readcnt == 0) 
        signal(wrt) // a writer can enter
    signal(mutex); //reader is done
} while(true);

Monitor is the collection of condition variables and procedures combined together in a special kind of module or a package. The process running outside the monitor can't access the internal variable of monitor but can call procedure of the monitor. Only one process at a time can execute code inside monitors.


## Java Integer overflow
Integer.MIN_VALUE + Integer.MIN_VALUE = 0
-Integer.MIN_VALUE = Integer.MIN_VALUE
Every bit including the sign bit obeys 2-complement law.
