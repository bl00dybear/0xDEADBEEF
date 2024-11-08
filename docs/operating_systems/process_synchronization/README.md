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

#### Kernel code (code implementing an operating system) is subject to several race conditions. 

Consider as an example a ```kernel data structure``` that mentain a ```list of all open files in system```. This list must be modified when a new file is opened or closed (adding the file to the list or removing it from the list). If two processes were to open files simultaneously, the separate updates to this list could result in a race condition.

Other ```kernel data structures``` that are prone to possible race conditions include ```structures for maintaining memory allocation```, for maintaining process lists, and for interrupt handling. It is up to kernel developers to ensure that the operating system is free from such race condition.