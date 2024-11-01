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