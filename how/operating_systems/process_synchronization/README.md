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

#### Kernel code (code implementing an operating system) is subject to several race conditions. 

Consider as an example a ```kernel data structure``` that mentain a ```list of all open files in system```. This list must be modified when a new file is opened or closed (adding the file to the list or removing it from the list). If two processes were to open files simultaneously, the separate updates to this list could result in a race condition.

Other ```kernel data structures``` that are prone to possible race conditions include ```structures for maintaining memory allocation```, for maintaining process lists, and for interrupt handling. It is up to kernel developers to ensure that the operating system is free from such race condition.