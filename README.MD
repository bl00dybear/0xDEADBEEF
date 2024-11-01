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

## How does code execute?

```Machine code is a low-level programming language consisting of binary instructions that a computer's CPU can execute directly.```

```
+----------------------------+
| 55 48 89 e5 48 8d 05 ac 0e |      Machine code instructions 
| 00 00 48 89 c7 e8 f0 fe ff |      (x64) that print “Hello, word!"
| ff b8 00 00 00 00 5d c3    |      to standard output.
+----------------------------+
```
### Now a good question is "Where the machine code comes from?"

```
                          From C code to execution

This is what processor execute                   The same but human readable
   +----------------------+                      +------------------------+
   | 55                   |    Instructions in   | push rbp               |
   | 48 89 e5             |    x64 assembly      | mov rbp, rsp           |
   | 48 8d 05 ac 0e 00 00 |    //          \\    | lea rax, [rip+0xeac]   |
   | 48 89 c7             |   //============\\   | mov rdi, rax           |
   | e8 f0 fe ff ff       |   \\============//   | call 1050 <puts@plt>   |
   | b8 00 00 00 00       |    \\          //    | mov eax, 0x0           |
   | 5d                   |    that print        | pop rbp                |
   | c3                   |    “Hello, world!”   | ret                    |
   +----------------------+    to the console    +------------------------+


  +------------------------+
  | push rbp               |
  | mov rbp, rsp           |      Compiling     +---------------------------+
  | lea rax, [rip+0xeac]   |   //               | int main(){               |
  | mov rdi, rax           |  //==============  |     puts("Hello, world!");|
  | call 1050 <puts@plt>   |  \\==============  |     return 0;             |
  | mov eax, 0x0           |   \\               | }                         |
  | pop rbp                |      process       +---------------------------+
  | ret                    |
  +------------------------+

```
