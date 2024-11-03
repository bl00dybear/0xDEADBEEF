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

