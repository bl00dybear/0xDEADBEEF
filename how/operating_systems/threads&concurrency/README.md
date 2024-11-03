```
████████╗██╗  ██╗██████╗ ███████╗ █████╗ ██████╗ ███████╗    
╚══██╔══╝██║  ██║██╔══██╗██╔════╝██╔══██╗██╔══██╗██╔════╝    
   ██║   ███████║██████╔╝█████╗  ███████║██║  ██║███████╗    
   ██║   ██╔══██║██╔══██╗██╔══╝  ██╔══██║██║  ██║╚════██║    
   ██║   ██║  ██║██║  ██║███████╗██║  ██║██████╔╝███████║    
   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═════╝ ╚══════╝    
```

## What is a thread?

A thread is a basic unit of CPU utilization; it comprises a thread ID, a program
counter (PC), a register set, and a stack.

It shares with other threads belonging to the same process its code section, data section, and other operating-system resources, such as open files and signals. 

A traditional process has a single thread of control. If a process has multiple threads of control, it can perform more than one task at a time.


```
Here is illustrated the difference between a traditional single-threaded process and a multithreaded process.

single-threaded process                           multi-threaded process 
         
+-------+-------+-------+             +-----------------------------------------------+
| code    data    files |             |         code        data         files        |
+-------+-------+-------+             +-----------------------+-----------------------+
|registers  PC    stack |             |        registers      |       registers       |
+-------+-------+-------+             +-----------------------+-----------------------+
|                       |             |          stack        |          stack        | 
|        thread         |             +-----------------------+-----------------------+
|                       |             |           PC          |           PC          | 
+-----------------------+             +-------+-------+-------+-------+-------+-------+
                                      |                       |                       |
                                      |         thread        |         thread        |
                                      |                       |                       |
                                      +-----------------------+-----------------------+
```

### As an example

Try this command : ```ps -ef```

Most operating system kernels are typically multithreaded. As an
example, during system boot time on Linux systems, several kernel threads
are created. Each thread performs a specific task, such as managing devices,
memory management, or interrupt handling. The command ps -ef can be
used to display the kernel threads on a running Linux system. Examining the
output of this command will show the kernel thread kthreadd (with pid = 2),
which serves as the parent of all other kernel threads.

### Mutithreadinf Models

Support for threads may be provided either at the user level, for ```user threads```, or by the kernel, for ```kernel threads```. User threads are supported above the kernel and
are managed without kernel support, whereas kernel threads are supported
and managed directly by the operating system. Ultimately, a relationship must exist between user threads and kernel threads. In this section, we look at three common
ways of establishing such a relationship: the many-to-one model, the one-to-
one model, and the many-to-many model.
```
                                      Many-to-One Model
+-----------------------------------+
|             user threads          |
|     /        /       /        /   |
|    (        (       (        (    |
|     \        \       \        \   |        The many-to-one model maps many user-level
|      )        )       )        )  |        threads to one kernel thread. Thread 
|     /        /       /        /   |        management is done by the library in user
|    (        (       (        (    |        space, so it is efficient.
|     \        \       \        \   |
|___________________________________|        However, the entire process will block if a 
|                 ||                |        thread makes a blocking system call.
|                 \/                |
|___________________________________|        Also, because only one thread can access
|                                   |        the kernel at a time, multiple threads are 
|                  /                |        unable to run in parallel on multicore 
|                 (                 |        systems.
|                  \                |
|                   )               |
|                  /                |
|                 (                 |
|                  \                |
|            kernel threads         |
+-----------------------------------+
 ```
 ```
                                       One-to-One Model
+-----------------------------------+
|             user threads          |
|     /        /       /        /   |
|    (        (       (        (    |
|     \        \       \        \   |        The one-to-one model maps each user thread 
|      )        )       )        )  |        to a kernel thread. It provides more
|     /        /       /        /   |        concurrency than the many-to-one model by 
|    (        (       (        (    |        allowing another 
|     \        \       \        \   |
|___________________________________|
|     ||       ||      ||       ||  |
|     \/       \/      \/       \/  |
|___________________________________|
|                                   |
|     /        /       /        /   |
|    (        (       (        (    |
|     \        \       \        \   |
|      )        )       )        )  |
|     /        /       /        /   |
|    (        (       (        (    |
|     \        \       \        \   |
|            kernel threads         |
+-----------------------------------+
```
```
+-----------------------------------+
|             user threads          |
|     /        /       /        /   |
|    (        (       (        (    |
|     \        \       \        \   |
|      )        )       )        )  |
|     /        /       /        /   |
|    (        (       (        (    |
|     \        \       \        \   |
|___________________________________|
|         ||       ||      ||       |
|         \/       \/      \/       |
|___________________________________|
|                                   |
|         /        /       /        |
|        (        (       (         |
|         \        \       \        |
|          )        )       )       |
|         /        /       /        |
|        (        (       (         |
|         \        \       \        |
|            kernel threads         |
+-----------------------------------+
```