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

### Mutithreading Models

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
|     /        /       /        /   |        The one-to-one model maps each user thread
|    (        (       (        (    |        to a kernel thread. It provides more
|     \        \       \        \   |        concurrency than the many-to-one model by 
|      )        )       )        )  |        allowing another thread to run when a
|     /        /       /        /   |        thread makes a blocking system call.
|    (        (       (        (    |         
|     \        \       \        \   |        It also allows multiple threads to run in
|___________________________________|        parallel on multiprocessors.
|     ||       ||      ||       ||  |         
|     \/       \/      \/       \/  |        The only drawback on this model is that
|___________________________________|        creating a user thread requires creating
|                                   |        the corresponding kernel thread, and a
|     /        /       /        /   |        large number of kernel threads may burden
|    (        (       (        (    |        the performance of a system. 
|     \        \       \        \   |        
|      )        )       )        )  |        Linux, along with the family of Windows
|     /        /       /        /   |        operating systems, implement the one-to-one
|    (        (       (        (    |        model.
|     \        \       \        \   |        
|            kernel threads         |        
+-----------------------------------+
```
```
                                      Many-to-Many Model
+-----------------------------------+
|             user threads          |
|     /        /       /        /   |        The many-to-many model multiplexes many
|    (        (       (        (    |        user-level threads to a smaller or equal 
|     \        \       \        \   |        number of kernel threads. The nuumber of 
|      )        )       )        )  |        kernel threads may be specific to either
|     /        /       /        /   |        a particular application or a particular
|    (        (       (        (    |        machine (an application may be allocated
|     \        \       \        \   |        more kernel threads on a system with eight
|___________________________________|        processing cores than a system with 4 cores).
|         ||       ||      ||       |
|         \/       \/      \/       |        Developers can create as many user threads 
|___________________________________|        as necessary, and the corresponding kernel 
|                                   |        threads can run in parallel on a 
|         /        /       /        |        multiprocessor.
|        (        (       (         |
|         \        \       \        |        When a thread performs a blocking system 
|          )        )       )       |        call, the kernel can schedule another thread
|         /        /       /        |        for execution.
|        (        (       (         |
|         \        \       \        |
|            kernel threads         |
+-----------------------------------+


One variation on the many-to-many model still multiplexes many user-level threads to a 
smaller or equal number of kernel threads but also allows a user-level thread to be bound
to a kernel thread. This variation is sometimes referred to as the two-level model.
```

### Thread Libraries

Before we proceed with our examples of thread creation, we introduce two general strategies for creating multiple threads: ```asynchronous threading``` and ```synchronous threading```. With ```asynchronous threading```, once the parent creates a child thread, the parent resumes its execution, so that the parent and child execute concurrently and independently of one another. Because the threads are independent, there is typically little data sharing between them. ```Asynchronous threading``` is the strategy used in the multithreaded server and is also commonly used for designing responsive user interfaces.

```Synchronous threading``` occurs when the parent thread creates one or more children and then must wait for all of its children to terminate before it resumes. Here, the threads created by the parent perform work concurrently, but the parent cannot continue until this work has been completed. Once each thread has finished its work, it terminates and joins with its parent. Only after all of the children have joined can the parent resume execution. Typically, synchronous threading involves significant data sharing among threads. For example, the parent thread may combine the results calculated by its various children.

```
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

int sum; /* this data is shared by the thread(s) */
void *runner(void *param); /* threads call this function */

int main(int argc, char *argv[])
{
   pthread t tid; /* the thread identifier */
   pthread attr t attr; /* set of thread attributes */
   
   /* set the default attributes of the thread */
   pthread attr init(&attr);
   
   /* create the thread */
   pthread create(&tid, &attr, runner, argv[1]);
   
   /* wait for the thread to exit */
   pthread join(tid,NULL);

   printf("sum = %d∖n",sum);
}

/* The thread will execute in this function */
void *runner(void *param)
{
   int i, upper = atoi(param);
   sum = 0;
   for (i = 1; i <= upper; i++)
      sum += i;
}
pthread exit(0);
}
```
### Implicit Threading

Implicit threading is a technique where the runtime system automatically manages the creation, scheduling, and synchronization of threads, allowing the programmer to express parallelism without manually handling threads.

1. Thread Pools

   The general idea behind a thread pool is to create a number of threads at start-up and place them into a pool, where they sit and wait for work. When a server receives a  request, rather than creating a thread, it instead submits the request to the thread pool  and resumes waiting for additional requests. If there is an available thread in the pool, it is awakened, and the request is serviced immediately. If the pool contains no  available thread, the task is queued until one becomes free. Once a thread completes its  service, it returns to the pool and awaits more work. Thread pools work well when the  tasks submitted to the pool can be executed asynchronously.

2. Fork Join

   In Fork Join strategy, the main parent thread creates (forks) one or more child threads and then waits for the children to terminate and join with it, at which point it can retrieve and combine their results. This synchronous model is often characterized as explicit thread creation, but it is also an excellent candidate for implicit threading. In the latter situation, threads are not constructed directly during the fork stage; rather, parallel tasks are designated. A library manages the number of threads that are created and is also responsible for assigning tasks to threads. In some ways, this fork-join model is a synchronous version of thread pools in which a library determines the actual number of threads to create

3. OpenMP

   OpenMP is a set of compiler directives as well as an API for programs written in C, C++, or FORTRAN that provides support for parallel programming in shared- memory environments. OpenMP identifies parallel regions as blocks of code that may run in parallel. Application developers insert compiler directives into their code at parallel regions, and these directives instruct the OpenMP run-time library to execute the region in parallel. The following C program illus- trates a compiler directive above the parallel region containing the printf() statement:
   ```
   #include <omp.h>
   #include <stdio.h>
   int main(int argc, char *argv[])
   {
      /* sequential code */
      #pragma omp parallel
      {
      printf("I am a parallel region.");
      }
      /* sequential code */
   }
   return 0;
   ```
   When OpenMP encounters the directive ```#pragma omp parallel``` it creates as many threads as there are processing cores in the system. Thus, for a dual-core system, two threads are created; for a quad-core system, four are created; and so forth. All the threads then simultaneously execute the parallel region. As each thread exits the parallel region, it is terminated.

   ```
   #pragma omp parallel for
   //parallelizing for loop
   for (i = 0; i < N; i++) {
      c[i] = a[i] + b[i];
   }
   ```

4. Grand Central Dispatch

   Grand Central Dispatch (GCD) is a technology developed by Apple for its macOS and iOS operating systems. It is a combination of a run-time library, an API, and language extensions that allow developers to identify sections of code (tasks) to run in parallel. Like OpenMP, GCD manages most of the details of threading. GCD schedules tasks for run-time execution by placing them on a dispatch queue. When it removes a task from a queue, it assigns the task to an available thread from a pool of threads that it manages.

5. Intel Thread Building Blocks

   Intel threading building blocks (TBB) is a template library that supports designing parallel applications in C++. As this is a library, it requires no special compiler or language support. Developers specify tasks that can run in parallel, and the TBB task scheduler maps these tasks onto underlying threads. Furthermore, the task scheduler provides load balancing and is cache aware, meaning that it will give precedence to tasks that likely have their data stored in cache memory and thus will execute more quickly. TBB provides a rich set of features, including templates for parallel loop structures, atomic operations, and mutual exclusion locking. In addition, it provides concurrent data structures, including a hash map, queue, and vector, which can serve as equivalent thread-safe versions of the C++ standard template library data structures.

### Threading Issuses
1. The ```fork()``` and ```exec()``` System Calls
   If one thread in a program calls ```fork()```, does the new process duplicate all threads, or is the new process single-threaded? Some UNIX systems have chosen to have two versions of ```fork()```, one that duplicates all threads and another that duplicates only the thread that invoked the ```fork()``` system call. The ```exec()``` system call typically works in the same way as described in the previous chapter. That is, if a thread invokes the ```exec()``` system call, the program specified in the parameter to ```exec()``` will replace the entire process—including all threads.

2. Signal Handling