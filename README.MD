```
██████╗ ██╗    ██╗███╗   ██╗
██╔══██╗██║    ██║████╗  ██║
██████╔╝██║ █╗ ██║██╔██╗ ██║
██╔═══╝ ██║███╗██║██║╚██╗██║
██║     ╚███╔███╔╝██║ ╚████║
╚═╝      ╚══╝╚══╝ ╚═╝  ╚═══╝
```

### What is binary exploitation?
```
-->breaking the boundaries on a compiled application
```
### What is the point?
```
-->get information off the machine 
-->execute arbitrary code
-->escalate to a higher privileged user
```

## Memory
### Memory layout of C program
```
   high address ______________________
                |                    |      command-line arguments
                |                    |      and environment variables
                |____________________|
                |       stack        |
                |--------------------|      grows downward
                |         |          |
                |         V          |
                |                    |
                |                    |
                |                    |
                |                    |
                |         Λ          |
                |         |          |
                |--------------------|      grows upwards
                |        heap        |
                |____________________|
                | uninitialized data |      initialized to 0
                |       (bss)        |      by exec
                |____________________|
                |  initialized data  |
                |____________________|      read from 
                |                    |      program file
                |        text        |      by exec
    low adress  |____________________|
                
    stack --> place where all the function parameters, return addresses
              and the local variables of the function are stored

    heap --> all the dynamically allocated memory resides here. (malloc)

```

### Registers

``` Registers are small, fast storage units inside the CPU that hold data and instructions needed for processing. ```
```
+----------------+---------------+---------------+-------------+
| 64-bit register| Lower 32 bits | Lower 16 bits | Lower 8 bits|
+----------------+---------------+---------------+-------------+
| rax            | eax           | ax            | al          |
| rbx            | ebx           | bx            | bl          |
| rcx            | ecx           | cx            | cl          |
| rdx            | edx           | dx            | dl          |
| rsi            | esi           | si            | sil         |
| rdi            | edi           | di            | dil         |
| rbp            | ebp           | bp            | bpl         |
| rsp            | esp           | sp            | spl         |
| r8             | r8d           | r8w           | r8b         |
| r9             | r9d           | r9w           | r9b         |
| r10            | r10d          | r10w          | r10b        |
| r11            | r11d          | r11w          | r11b        |
| r12            | r12d          | r12w          | r12b        |
| r13            | r13d          | r13w          | r13b        |
| r14            | r14d          | r14w          | r14b        |
| r15            | r15d          | r15w          | r15b        |
+----------------+---------------+---------------+-------------+
```

```
    The structure of a register
+----------------------------------+
|               |        | ah | al |    --> 8 bits eatch
+----------------------------------+
                         |____ax___|    --> 16 bits
                |_______eax________|    --> 32 bits
|______________rax_________________|    --> 64 bits
```
```
                            Common Registers

--> %rip – instruction pointer registers
    • it stores the address of the next instruction to be executed
--> %rsp – stack pointer register
    • it stores the address of the top of the stack.
--> %rbp – base pointer register
    • keeps tab of function parameters and local variables
```