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

Let’s consider an example of how this can happen.

A producer process produces information that is consumed by a consumer process. For example, a compiler may produce assembly code that is consumed by an assembler. The assembler, in turn, may produce object modules that are consumed by the loader. The producer–consumer problem also provides a useful metaphor for the client–server paradigm.

One solution to the producer–consumer problem uses shared memory. To allow producer and consumer processes to run concurrently, we must have available a buffer of items that can be filled by the producer and emptied by the consumer. Two types of buffers can be used. The ```unbounded buffer``` places no practical limit on the size of the buffer. The consumer may have to wait for new items, but the producer can always produce new items. The ```bounded buffer``` assumes a fixed buffer size. In this case, the consumer must wait if the buffer is empty, and the producer must wait if the buffer is full.

```
/               *Producer code*/
while (true) {
        /* produce an item in next produced */
        while (counter == BUFFER SIZE)
                ; /* do nothing */
        }
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
        }
        /* consume the item in next consumed */
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