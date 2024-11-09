```
______                                                                     
| ___ \                                                                    
| |_/ / __ ___   ___ ___  ___ ___                                          
|  __/ '__/ _ \ / __/ _ \/ __/ __|                                         
| |  | | | (_) | (_|  __/\__ \__ \                                         
\_|  |_|  \___/ \___\___||___/___/                                         
                                                                        
 _____                  _                     _          _   _             
/  ___|                | |                   (_)        | | (_)            
\ `--. _   _ _ __   ___| |__  _ __ ___  _ __  _ ______ _| |_ _  ___  _ __  
 `--. \ | | | '_ \ / __| '_ \| '__/ _ \| '_ \| |_  / _` | __| |/ _ \| '_ \ 
/\__/ / |_| | | | | (__| | | | | | (_) | | | | |/ / (_| | |_| | (_) | | | |
\____/ \__, |_| |_|\___|_| |_|_|  \___/|_| |_|_/___\__,_|\__|_|\___/|_| |_|
        __/ |                                                              
       |___/                                                               
```

#### A cooperating process is one that can affect or be affected by other processes executing in the system. Cooperating processes can either directly share a logical address space (that is, both code and data) or be allowed to share data only through files or messages.

Let’s consider an example to highlight the need for process synchronization.

A producer process produces information that is consumed by a consumer process. For example, a compiler may produce assembly code that is consumed by an assembler. The assembler, in turn, may produce object modules that are consumed by the loader. The producer–consumer problem also provides a useful metaphor for the client–server paradigm.

One solution to the producer–consumer problem uses shared memory. To allow producer and consumer processes to run concurrently, we must have available a buffer of items that can be filled by the producer and emptied by the consumer. Two types of buffers can be used. The ```unbounded buffer``` places no practical limit on the size of the buffer. The consumer may have to wait for new items, but the producer can always produce new items. The ```bounded buffer``` assumes a fixed buffer size. In this case, the consumer must wait if the buffer is empty, and the producer must wait if the buffer is full.

```
/               *Producer code*/
while (true) {
        /* produce an item in next produced */
        while (counter == BUFFER SIZE)
                ; /* do nothing */
        
        buffer[in] = next produced;
        in = (in + 1) % BUFFER SIZE; //buffer is implementea as a circular array
        counter++;
}
```
```
                /*Consumer code*/
while (true) {
        while (counter == 0)
                ; /* do nothing */
        next consumed = buffer[out];
        out = (out + 1) % BUFFER SIZE;
        counter--;
        /* consume the item in next consumed */
}
```

These two processes running concurrently can result in a race condition. Let's consider that we start with the counter on the value 5. The concurrent execution of “counter++” and “counter--” is equivalent to a sequential execution in which the lower-level statements are interleaved in some arbitrary order (but the order within each high-level statement is preserved). One such interleaving is the following:
```
T0 :    producer        execute         register1 = counter             {r egister1 = 5}
T1 :    producer        execute         register1 = register1 + 1       {r egister1 = 6}
T2 :    consumer        execute         register2 = counter             {r egister2 = 5}
T3 :    consumer        execute         r egister2 = r egister2 − 1     {r egister2 = 4}
T4 :    producer        execute         counter = r egister1            {counter = 6}
T5 :    consumer        execute         counter = r egister2            {counter = 4}
```
Notice that we have arrived at the incorrect state “counter == 4”, indicating that four buffers are full, when, in fact, five buffers are full. If we reversed the order of the statements at T4 and T5 , we would arrive at the incorrect state “counter == 6”. We would arrive at this incorrect state because we allowed both processes to manipulate the variable counter concurrently.

#### The Critical-Section Problem

We begin our consideration of process synchronization by discussing the so-called critical-section problem. Consider a system consisting of n processes {P0 , P1 , ..., Pn−1 }. Each process has a segment of code, called a critical section, in which the process may be changing common variables, updating a table, writing a file, and so on. The important feature of the system is that, when one process is executing in its critical section, no other process is allowed to execute in its critical section. That is, no two processes are executing in their critical sections at the same time. The critical-section problem is to design a protocol that the processes can use to cooperate. Each process must request permission to enter its critical section. The section of code implementing this request is the entry section. The critical section may be followed by an exit section. The remaining code is the remainder section. The general structure of a typical process Pi is shown below:
```
do {
        +---------------+
        | entry section |
        +---------------+

                critical section

        +--------------+
        | exit section |
        +--------------+

                remainder section

} while (true);
```
A solution to the critical-section problem must satisfy the following three requirements:
1. ```Mutual exclusion.``` If process Pi is executing in its critical section, then no other processes can be executing in their critical sections.
2. ```Progress.``` If no process is executing in its critical section and some processes wish to enter their critical sections, then only those processes that are not executing in their remainder sections can participate in deciding which will enter its critical section next, and this selection cannot be postponed indefinitely.
3. ```Bounded waiting.``` There exists a bound, or limit, on the number of times
that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that
request is granted.


#### Kernel code (code implementing an operating system) is subject to several race conditions. 

Consider as an example a ```kernel data structure``` that mentain a ```list of all open files in system```. This list must be modified when a new file is opened or closed (adding the file to the list or removing it from the list). If two processes were to open files simultaneously, the separate updates to this list could result in a race condition.

Other ```kernel data structures``` that are prone to possible race conditions include ```structures for maintaining memory allocation```, for maintaining process lists, and for interrupt handling. It is up to kernel developers to ensure that the operating system is free from such race condition.

Two general approaches are used to handle critical sections in operating systems: ```preemptive kernels``` and ```nonpreemptive kernels```. A ```preemptive kernel``` allows a process to be preempted while it is running in kernel mode. A ```nonpreemptive kernel``` does not allow a process running in kernel mode to be preempted; a kernel-mode process will run until it exits kernel mode, blocks, or voluntarily yields control of the CPU.

### Peterson's Solution
Peterson’s solution is restricted to two processes that alternate execution between their critical sections and remainder sections. The processes are numbered P0 and P1 . For convenience, when presenting Pi , we use Pj to denote the other process; that is, ```j``` equals ```1 − i```.
```
do {
        flag[i] = true;                 --+
        turn = j;                         | entry section
        while (flag[j] && turn == j);   --+
        
                critical section
        
        flag[i] = false;
        
        remainder section               | exit section
} while (true);
```

### Synchronization Hardware

Many modern computer systems therefore provide special hardware instructions that allow us either to test and modify the content of a word or to swap the contents of two words atomically—that is, as one uninterruptible unit. We can use these special instructions to solve the critical-section problem in a relatively simple manner. Rather than discussing one specific instruction for one specific machine, we abstract the main concepts behind these types of instructions by describing the test and set() and compare and swap() instructions.

The test and set() instruction can be defined below:
```
boolean test and set(boolean *target) {
    boolean rv = *target;
    *target = true;
    return rv;
}
```
```
do {
    while (test and set(&lock))
        ; /* do nothing */

    /* critical section */
    
    lock = false;
    /* remainder section */
} while (true);
```

This code demonstrates a simple spinlock mechanism using the test_and_set function for mutual exclusion.

```test_and_set``` function: This function takes a pointer to a boolean variable (target) and performs an atomic operation. It saves the current value of *target in rv, sets *target to true (indicating the lock is acquired), and returns the original value. If *target was true, it means the lock was already held by another process.

```Main loop```:

The while (test_and_set(&lock)) loop continuously checks if the lock is available. If test_and_set returns true, the process waits; if it returns false, the lock is acquired.

```Critical Section```: After obtaining the lock, the code enters the critical section, where it performs operations that require exclusive access.

```Release Lock```: After the critical section, lock is set to false to release the lock, allowing other processes to enter the critical section.

This process repeats indefinitely due to the outer ```do...while(true)``` loop.
```
/*An example of Test nd set method in practice*/

#include <stdio.h>
#include <pthread.h>
#include <stdatomic.h>

// This flag will act as the lock indicator.
// "volatile" tells the compiler that this variable can be changed by other threads at any time,
// preventing optimizations that might interfere with correct operation.
volatile int lock_flag = 0; // lock_flag initialized to 0 (unlocked state)

// Lock function that implements the test-and-set behavior
void lock() {
    // __sync_lock_test_and_set() atomically sets lock_flag to 1 and returns the previous value of lock_flag.
    // If the previous value was 1, it means the lock is held by another thread, so this thread must wait.
    // The while loop repeatedly checks until the lock_flag is 0, allowing this thread to "acquire" the lock.
    while (__sync_lock_test_and_set(&lock_flag, 1)) {
        // Spin-wait (busy-waiting) until the lock becomes available.
    }
}

// Unlock function to release the lock
void unlock() {
    // __sync_lock_release() sets lock_flag back to 0 (unlocked),
    // allowing other threads to acquire the lock.
    __sync_lock_release(&lock_flag);
}

// Shared resource that needs protection by the lock
int shared_counter = 0;

// Function executed by each thread to increment the shared_counter
void* increment(void* arg) {
    // Each thread will try to increment the counter 100,000 times.
    for (int i = 0; i < 100000; ++i) {
        lock();               // Acquire the lock before entering critical section
        shared_counter++;     // Critical section: safely increment shared counter
        unlock();             // Release the lock after leaving critical section
    }
    return NULL;
}

int main() {
    pthread_t t1, t2; // Thread identifiers for two threads

    // Create two threads, each running the increment function
    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);

    // Wait for both threads to finish their execution
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    // Print the final value of the shared_counter
    printf("Final counter value: %d\n", shared_counter);
    return 0;
}
```