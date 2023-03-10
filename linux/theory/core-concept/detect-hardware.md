# Communicating with Hardware in Linux System

The **` device driver `** is the abstraction layer between software concepts and hardware circuitry as such, it needs to talk with both of them. In this chapter we will discuss, how a driver can access **` I/O ports `** and **` I/O memory `**. 

### Devices accessing or communicating approach

  - During earlier days, the address bus and data bus was smaller. Address bus used to 16 bits wide and data bus used to be 8 bits wide.
  - Further, compare to the CPU speep the peripherals used to work at a slower speed.
  - As a result, some of the manufacturers decided to have a separate address space for the **_devices_** and for **_memory ( RAM )_**. This approach is called **` I/O Port `** access.
  - And others decided to have a single address space for both  **_peripherals_** and **_memory_**. This approach is called **` I/O Memory `** access.
  - The x86 family of processors have separate **_read_** and **_write_** electrical lines for **` I/O ports `** and special CPU instruction to access ports.
  - **_Linux Kernel_** supports both the concepts on all the platforms.
  - The use of **_I/O Ports_** is common for ISA peripherals. e.g; parallel port.
  - The use of **_I/O Memory_** is common for PCI devices. e.g; video card.
  - The **_I/O Memory_** approach is generally preferred because of:
    - It doesn't require the use of special -purpose processor instructions.
    - CPU's access memory much more efficiently.
    - Compilers can optimize the code for better register allocation and addressing mode selection when accessing memory.
 

### Registers in hardware device
 - All the devices are controlled by reading and writing hardware registers.
 - Most of the time a device has several registers and communication with a device involves access to more than one register.
 - Each device has minimum of the below registers:
   - A data register (either readable or writable, depending on whether it is an input or output device)
   - A control register (writable, for controlling device operation)
   - A stste register (readable, for determining device status e.g., whether it is ready to receive or provide data)
 - More complex devices e.g., disks have multiple control and status registers.

### Distinguish between I/O port and I/O memory access
On **_x86_** machine there is a separate pin **_IO/M_**. This pin is used to distinguish between **_memory_** and **_I/O_** operations.
  - High ---> I/O operation
  - Low  ---> Memory operation.
When **_IN/OUT_** instruction signal is used, the **_IO/M_** pin is high, so the memory doesn't respond and **_I/O_** chip does. <br>
And when **_LOAD/STORE_** instructions is used, the **_IO/M_** pin is low and the memory responds and **_I/O_** devices stay out of communication.

#### Access memory 
**_Address Bus:_** Identify the source or destination of data. <br>
**_Data Bus:_** Carries data <br>
**_Control Bus:_** Control and timing information (read/write, clock signals)

![system bus](../../images/core-concept/connect-device/system-bus.png)

  - CPU puts the address of memory on the address bus
  - Sets the control signal indicating read/write operation
  - Data is placed by the corresponding memory chip on the data bus.

**_Note:_** <br>
Now a days, this is handled by the memory controller. CPU communicates with the memory controller.

### Connecting device to running machine

Whenever we inserted the devices to the running machine, dynamically a file gets created for that particular device and generally it shows into the **` /dev/ `** directory with some meaningfull name.

Suppose we require to connect **` USB Device `** to the running machine or system. Then **` kernel `** quitely does their job internally and executes the various services to acheve the goals. 
  - Post inserting the **` USB Device `** to the running system or machine the corresponding **device driver** which resides in the **` kernel space `** detects the state change and generates the events which is **` uevents `**.
  - The **` udev `** daemon, **systemd-udevd.service**, receives device **` uevents `** directly from the kernel whenever a device is added or removed from the system or it changes its state.
  - When **` udev `** receives a device event, it matches its configured set of rules against various device attributes to identify the device.
  - **` udev `** supplies the system software with device events, manages permissions of device nodes and may create additional symlinks in the **` /dev/ `** directory.
  - The **` udev `** rules are read from the files located in the directories **` /usr/lib/udev/rules.d `** and **` /usr/local/lib/udev/rules.d `**, the volatile runtime directory **` /run/udev/rules.d `** and the local administration directory **` /etc/udev/rules.d `**.
  - Files in **` /etc/ `** have the highest priority.
  - Once the process is completed the newly inserted or attached device will shwon in the **` /dev/ `** directory with dynamic name created by **` udev `** Dynamic Device Management. 
  - **` udev `** is basically a piece of code or a **User Space** program, which receives signal or notification from **` kernel `**.

  <img src="../../images/core-concept/connect-device/attach-device.png" height="550" width="800" >
  
  Now we can see the attached device under **` /dev/ `** directory and can verify by executing below commands:
  ```
  # lsblk
  # df -hT
  ```
  ![device connect](../../images/core-concept/connect-device/connect-usb.png)
  
### Operators in udev rules file
| **Operators** | **Description**                                                 |
|---------------|-----------------------------------------------------------------|
| **` == `**    | Compare for equality.                                           |
| **` != `**    | Compare for inequality.                                         |
| **` = `**     | Assign a value to a key.                                        |
| **` += `**    | Add the value to a key that holds a list of entries.            |
| **` -= `**    | Remove the value from a key that holds a list of entries.       |
| **` := `**    | Assign a value to a key finally; disallow any later changes.    |

### **Detect Device Connectivity Status using *` dmesg `* command**
**` dmesg `** can be used to display the message of **` Kernel Ring Buffer `** generated by the **kernel**. The numerous messages generated by the **` kernel `** whenever the linux based system boots up and this is also contains logs which is produced by the hardware devices that the kernel detects and indicate whether it is able to configure them. To identfy or display **` kernel ring buffer `** messages/logs execute the below command.
```
# dmesg
```
**OR** to read in human readable form and with standard date and time.
```
# dmesg -HTx
```
![kernel ring buffer](../../images/core-concept/connect-device/usb-dmesg.png)

A level is assigned to each message logged to the *` kernel ring buffer `*. The level represents the significance of the information in the communication. The levels are as follows:
| **levels**    | **Description**                                                                      |
|---------------|-----------------------------------------------------------------------|
| **` emerg `** | In this situation not able to use system.                             |
| **` alert `** | Something happened wrong and immediate action required.               |
| **` crit `**  | Critical condition occured like hardware or software related failure. |
| **` err `**   | An error occured related to hardware                                  |
| **` warn `**  | Nothing serious but might indicate some issue.                        |
| **` notice `**| Often report security events.                                         |
| **` info `**  | Startup informational message.                                        |
| **` debug `** | Debug message.                                                        |


The **` dmesg `** command extract the messages that match a particular **level** by using the **` -l `** option and passing the name of the level as a command-line argument.<br>
**Syntax:**
```
# dmesg -l level_name
```
Replace the level_name with desired level.<br>
**Example:**
```
# dmesg -l info
```
![dmesg facility](../../images/core-concept/connect-device/dmesg-level.png)

To see more than one level based message execute the below command:
```
# dmesg -l info,notice
```
The **` dmesg `** messages are grouped into categories called **facilities**. The list of facilities are as follow:

| **Facilities**  | **Description**                  |
|-----------------|----------------------------------|
| **` kern `**    | Kernel message.                  |
| **` user `**    | User-level message.              |
| **` mail `**    | Logs related to mail system.     |
| **` daemon `**  | Message related to System daemon |
| **` auth `**    | Security and authorization message. |
| **` syslog `**  | Internal syslogd message.           |
| **` lpr `**     | Line printer sub-system.            |
| **` news `**    | Network news sub-system.            |

The **` dmesg `** can filter its output to only show messages in a specific facility. To do so, we must use the **` -f `** option:<br>
**Example:**
```
# dmesg -f daemon
```
![dmesg facility](../../images/core-concept/connect-device/dmesg-facility.png)

We can combine the both **` facility `** and **` level `** togather using **` -x `** option:
```
# dmesg -x
```
![level & facility](../../images/core-concept/connect-device/level-facility.png)

**_Watching live message_** <br>
To see messages as they arrive in the kernel ring buffer, use the **` --follow `** (wait for messages) option.
```
# dmesg --follow
```


### The **` udevadm `**
The **` udevadm monitor `** is a device management tool in linux based system which manages all the device events and it controls the runtime behavior of **` udev `**, requests kernel events, manages the event queue, and provides simple debugging mechanisms.
```
# udevadm info --query=path --name=/dev/sdc1
```
The **` udevadm `** listens to the kernel new uevents upon detecting an event, it displays the details of newly attached or removed device such as the **` device path `** and the **` device name `** on the teminal and so on.
```
# udevadm monitor
```
![level & facility](../../images/core-concept/connect-device/udevadm-monitor.png)

### Peripheral Connect Interconnect (PCI)
Peripheral Connect Interconnect is a local computer bus for attaching hardware devices in a computer system and is part of the **` PCI Local Bus `** standard. We can connect **` Wi-Fi Adaptor `**, **` Ethernet Card `**, **` Sound Card `**, **` Graphics Card `**, **` RAID Controler `**, etc directly to the **` PCI Slots `** on the motherboard of the compter system.
<img src="../../images/core-concept/connect-device/pci-connect.png" height="550" width="900">

To list the connected devices to the PCI slots in the system execte below command:
```
# lspci
```
![pci slots](../../images/core-concept/connect-device/pci-devices.png)

### Peripheral Devices
A peripheral device is any auxiliary device that connects to and works with the computer to either put information into it or get information out of it. There are generally two types of peripheral devices which is **Internal Peripheral** and **External Peripheral** devices and these peripheral devices can be either **` Input Device `** or **` Output device `** or **` Storage Device `**. Few examples of peripheral devices are:
  - Network Interface Card
  - Video Card
  - Wi-Fi Adaptor
  - Keyboard & Mouse
  - RAID Controler
  - Monitor and many more

To list the information about connected **` Block Device `** to the system execute the command below:
```
# lsblk
```
![block device info](../../images/core-concept/connect-device/lsblk-result.png)

The output of **` lsblk `** command arragned with various fields which is listed below: <br>
**1. NAME**<br>
The first field is NAME, which shows the device name. <br>
**2. MAJ:MIN** <br>
This field respectively, indicates the major and minor device numbers. <br>
  - **Major Number:** Indicates the device type
  - **Minor Number:** Indicates the device

**3. RM** <br>
This displays boolean values for removable and non-removable devices.
  - 1  ---> Removal Device
  - 0  ---> Non-removal Device

**4. SIZE** <br>
This field displays the device size in a readable format, i.e., in K, M, G, T, etc. <br>
**5. RO** <br>
This field shows the read-only status of a device.
  - 1  ---> Read-only Device
  - 0  ---> No read-only Device

**6. TYPE** <br>
This field shows the type of devices, such as disk, loopback device, partition, or LVM device. <br>
**7. MOUNTPOINT** <br>
This displays the mount point on which the device is mounted.


| **Major Number** | **Device Type** | **Minor Number** | **Device**        | **Description**                     |
|------------------|-----------------|------------------|-------------------|-------------------------------------|
| **` 1 char `**   | Memory device   | **` 1 `**        | **` /dev/mem `**  | Physical memory access              |
|                  |                 | **` 2 `**        | **` /dev/kmem `** | OBSOLETE - replaced by /proc/kcore  |
|                  |                 | **` 3 `**        | **` /dev/null `** | Null device                         |
|                  |                 | **` 4 `**        | **` /dev/port `** | Input Output port access.           |
|                  |                 | **` 5 `**        | **` /dev/zero `** | Null byte source                    |
|                  |                 | **` ... `**      |                   | so on                               |
| **` 1 block `**  | RAM Disk        | **` 0 `**        | **` /dev/ram0 `** | First RAM disk                      |
|                  |                 | **` 1 `**        | **` /dev/ram1 `** | Second RAM disk                     |
|                  |                 | **` ... `**      |                   | so on                               |
| **` 2 char `**   | Pseudo-TTY masters | **` 0 `**     | **` /dev/ptyp0 `**| First PTY master                    |
|                  |                    | **` ... `**   |                   | so on                               |
| **` 2 block `**  | Floppy disks       | **` 0 `**     | **` /dev/fd0 `**  | Controller 0, drive 0, autodetect   |
|                  |                    | **` ... `**   |                   | so on                               |
| **` 3 char `**   | Pseudo-TTY slaves  | **` 0 `**     | **` /dev/ttyp0 `**| First PTY slave                     |
|                  |                    | **` 1 `**     | **` /dev/ttyp1 `**| Second PTY slave                    |
|                  |                    | **` ... `**   |                   | so on                               |
| **` 3 block `**  | IDE hard disk/CD-ROM interface | **` 0 `**  | **` /dev/hda `**     | Master: whole disk or CD-ROM |
|                  |                                | **` 64 `** | **` /dev/hdb `**     | Slave: whole disk or CD-ROM  |
|                  |                                | **` 1 `**  | **` /dev/hd(a/b)1 `**| First partition              |
|                  |                                | **` ... `**|                      | so on                        |
| **` 4 char `**   | TTY devices        | **` 0 `**     | **` /dev/tty0	`** | Current virtual console                  |
|                  |                    | **` ... `**   |                   | so on upto 63                            |
|                  |                    | **` 64 `**    | **` /dev/ttyS0 `**| First UART serial port                   |
|                  |                    | **` ... `**   |                   | so on upto                               |
| **` 6 `**        | Parallel printer devices       | **` 0 `**  | **` /dev/lp0 `**     | Parallel printer on parport0 |
|                  |                                | **` ... `**|                      | so on                        |
| **` 7 char `**   | Virtual console capture devices| **` 0 `**  | **` /dev/vcs `**     | Current vc text contents     |
|                  |                                | **` ... `**|                      | so on                        |
| **` 7 block `**  | Loopback devices               | **` 0 `**  | **` /dev/loop0 `**   | First loop device            |
|                  |                                | **` ... `**|                      | so on                        |
| **` 8 block `**  | SCSI disk devices              | **` 0 `**  | **` /dev/sda `**     | First SCSI disk whole disk   |
|                  |                                | **` 16 `** | **` /dev/sdb `**     | Second SCSI disk whole disk  |
|                  |                                | **` 32 `** | **` /dev/sdc `**     | Third SCSI disk whole disk   |
|                  |                                | **` ... `**|                      | so on                        |
| **` 9 block `**  | Metadisk (RAID) devices        | **` 0 `**  | **` /dev/md0 `**     | First metadisk group         |

And so on, there are each and every device in linux based system has **` Major `** and **` Minor `** number pair.

### List Out CPU Information
To list the detail **` CPU `**  information such as their architecture, vendor, family, model, CPU caches, etc. execute the below command:
```
# lscpu
```
![cpu info](../../images/core-concept/connect-device/cpu-info.png)

### List the Memory Information
To list the ranges of available memory with their online status and detail information of memory execute the below command.
```
# lsmem
```
**OR**
```
# free -h
```
![memory info](../../images/core-concept/connect-device/memory-info.png)

### Fetching Hardware details
To list to hardware details we can make use of some command or tools which are **` lshw `** and **` dmidecode `**. These tools display the description of the system's  hardware  components,  as  well  as other  useful pieces of information such as serial numbers, BIOS revision, etc.
```
# lshw
```
![hardware info](../../images/core-concept/connect-device/hardware-info.png)
**OR**
```
# dmidecode
```
![hardware info](../../images/core-concept/connect-device/hardware-detail.png)

For more information read the manual page of commands as below:
```
# man lshw
# man dmidecode
# man lscpu
# man 8 udev
# man udevadm
```


