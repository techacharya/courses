# Process Management and Monitoring in `linux` System
A process is basically a program in the execution. _For example:_ Suppose we have a **_C language_** program file _**` welcome.c `**_ and this program file will be given to the **_C language Compiler_** like _**` gcc `**_. Compiler will just convert this **_high level language_** (source code) to **_low level language_** (machine code) that is a binary code or file, which is directly executes on the hardware and provides the appropriate result.

The original code and binary files or codes both are called program only and generally reside in the secondary storage device i.e., **_HDD/SSD_**. In order to exeute this program Operating System loads this program into the **_main memory_** i.e., **_RAM_** and starts executing it. When program loads into **_main memory_** it creates data structures into the **_RAM_** and this structure is called as process.

![Process Structure](../../images/process-mgmt/peocess-structure.png)

  - **_Executables/text_**

    This section of memory contains the executable codes which are being executed or the current activity represented by the value of the **_Program Counter_**.
  - **_Static Local or Global variable_**

    This section contains all the **_local_** and **_global_** variables defined in the program. 
  - **_Stack_**

    Most of the program has the function calls or recursion . So the stack require to contain the temporary data, such as function parameters, returns addresses, and local variables.
  - **_Heap_**

    Program also has dynamic array, linked lists, etc. So heap requires to conatin the dynamically allocated memory to process during its execution time. 
    
This structure of memory is known as process boundary and eveythig required by the particular process will lies in this boundary only. While **_Operating System_** executing this particular process should never cross these boundaries. This boundary is called process.

**A process consists of:** 
  - An address space of allocated memory
  - Security properties including ownership, credentials and privileges
  - One or more threads of program code
  - Process states

**The environment of process includes:**
  - Golbal and local variables
  - A current scheduling context
  - Allocated system resources, such as file descriptors and network ports, etc.

### Identification of Process
There are number of process created in the system randomaly at any point of time in that senerio we are supposed to keep track of which process is what, how many process are there and what is the current state of a particular process. So to identify a particular process among various process there is a set of attribute are recorded at specific location called **_Process Control Block_** or **_PCB_**. <br>
**_Attributes of Process:_** <br>
  - **_Process ID_**
  - **_Program Counter_**
  - **_Process State_**
  - **_Priority_**
  - **_General Purpose Register_**
  - **_List of Open Files_**
  - **_List of Open Devices_**
  - **_Protection_**

  <img src="../../images/process-mgmt/pcb.png" width="450" height="550">

#### Process ID
A unique numerical identification number assigned by the operating system to the process.
#### Program Counter
It is a register in **_CPU_** and contains that memory address from where the next instruction is to be fetched for execution.
#### Process State
The state of a process is the current activity of the process. The process states are:
  - **_New_**
  - **_Ready_**
  - **_Running_**
  - **_Block or Wait_**
  - **_Termination or Completion_**
  - **_Suspend ready_**
  - **_Suspend wait or Suspend block_**
#### Priority
It shows the importance of the process and generally it is a number.
#### General Purpose Register
The intermediate result present in registers is keep tracked while process is preempted or moved to waiting state.
#### List of Open Files
During the execution of process files are open for reading or writing purpose. So it keeps records of all the open files.
#### List of Open Devices
It keeps records of all open devices like printers, disk, etc.
#### Protection
It defines the area of memory allocated for the process. Any process should not access the other process's allocated memory space.


**_Operating System_** creates a _**` Process Control Block `**_ PCB for each and every process and this **_PCB_** contains all the process attributes to manages it. These all **_PCBs_** are represented in the linked list. In the linux kernel, each process is represented by a _**` task_struct `**_ in a **_doubly-linked list_**, the head of which is _**` init_task `**_.

![Process Control Block](../../images/process-mgmt/pcb-list.png)

**Context Switching** <br>
This **_Process Control Block_** is also known as context. The process of saving the **_context_** of one process and loading the **_context_** of another process is called _**` Context Switching `**_. This may happen when:
  - _A high-priority process comes to a ready state than the running process_
  - _An Interrupt occurs_
  - _User-mode and kernel-mode switch_
  - _Preemptive CPU scheduling used._


These all information related to process can be seen in the directory _**` /proc/PID/ `**_.
![Process Control Block](../../images/process-mgmt/process-info.png)

### Process Types
There are two types of process in linux system:
  - **_User Process_**
  - **_System Process_**

#### User Process
User processes are executed in user mode and can be preempted or suspended at the time of executing. It executes the user code.
#### System Process
System processes are executed in privileged mode and executed automatically as per requirement without preemption. It executes the system code.

### Ways of running Process
There are two ways to run the process in linux system:
  - **_Foreground Process_**
  - **_Background Process_**

#### Foreground Process
Processes that require a **_user_** to start them or to interact with them are called _foreground processes_. It needs certain inputs from the user and gives some output on the screen. Such way of running processes are also known as _interactive processes_. Programs and commands run as foreground processes by default.

#### Background Process
Processes that are run independently of a user are referred to as background processes. To run a process in the background, place an ampersand **_(&)_** symbol at the end of the command name that you use to start the process. Such ways of running processes are also known as _non-interactive processes_.

### Managing the Processes
Now, going to start the _**` clock.sh `**_ process . This approach allows to see the process without making meaningful changes to my system. Write the simple script of simple clock create a **` shell script `** file _cloock.sh_ with following content. To do so copy the bellow content and paste into the the **_clock.sh_** file.
```
$ vim clock.sh
```
```
#!/usr/bin/env bash

while true
do
	columns=`tput cols`
	nc=`echo $(( $columns/2 ))`
	rows=`tput lines`
	nr=`echo $(( $rows/2))`
	mid=`echo $(( $columns*$nr+$nc-4 ))`

	clear
	printf '%*s' "$mid"
	echo -e "\e[1;31m `date +%H:%M:%S`\e[0m"
	sleep 1
done
```
Post writing into the file escape from the **` vim `** editer by pressing _**` Esc `**_ key then type key combination **`:wq!`** to save and exit. Set the execution permission to the file _**` clock.sh `**_ by **` chmod +x clock.sh `** command and start the process by executing following command: 
```
$ ./clock.sh
```
Then stop the process with _**` Ctrl + Z `**_ so that we can use our terminal.

#### List processes
To list all currently active processes execute the following command:
```
$ ps
```
![list process](../../images/process-mgmt/list-process.png)

Here, in the above figure you can see few information about the active process on the system.
| **_Field_** | **_Description_**                                           |
|-------------|-------------------------------------------------------------|
| **_PID_**   | Unique process ID                                           |
| **_TTY_**   | A pseudo device that allows us to interact with the system. |
| **_TIME_**  | The amount of time that the process has been running        |
| **_CMD_**   | The command executed to launch the process                  |

#### List process with more details
To see the more details about active process the options can be used. Execute the following command to list with more details:
```
$ ps -au
```
![ps command options](../../images/process-mgmt/ps-options.png)

In the above figure of **` ps `** command output, the _**` STAT `**_ column or field indicates the current state of each process.

| **_State_**  | **_Description_**                                                            |
|--------------|------------------------------------------------------------------------------|
| **_D_**      | Uninterruptible sleep (usually IO)                                           |
| **_I_**      | Idle kernel thread                                                           |
| **_R_**      | running or runnable (on run queue)                                           |
| **_S_**      | Interruptible sleep (waiting for an event to complete)                       |
| **_T_**      | Stopped by job control signal                                                |
| **_t_**      | Stopped by debugger during the tracing                                       |
| **_W_**      | Paging (not valid since the 2.6.xx kernel)                                   |
| **_X_**      | Dead (should never be seen)                                                  |
| **_Z_**      | Defunct or _**` zombie `**_ process, terminated but not reaped by its parent |

For **_BSD_** formats and when the **_stat_** keyword is used, additional characters may be displayed:
| **_Symbol_** | **_Description_**                                                              |
|--------------|--------------------------------------------------------------------------------|
| **` < `**    | High-priority (not nice to other users)                                        |
| **` N `**    | The low-priority (nice to other users)                                         |
| **` L `**    | Has pages locked into memory (for real-time and custom IO)                     |
| **` s `**    | It is a session leader                                                         |
| **` l `**    | It is multi-threaded                                                           |
| **` + `**    | is in the foreground process group                                             |

The **` ps `** command options and it accepts several kinds of options:
  - **_UNIX Options:_** Which may be grouped and must be preceded by a dash ( **` - `** ).
  - **_BSD Options:_** It may be grouped and must not be used with a dash.
  - **_GNU long Options:_**, which are preceded by two dashes ( **` -- `** ).

