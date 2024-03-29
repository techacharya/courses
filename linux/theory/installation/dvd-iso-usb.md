# Install Linux Operating system On Stand-alon Hardware

In this lession we will learn to install linux based operationg system. Installing linux based OS on a **_Server_**, **_Desktop_** or **_laptop_** is easier than you think.

To install Linux on a computer or laptop, you will need:
  - A suitable target device like, **_Server, Virtual Machine, Desktop, Laptop_**
  - A downloaded linux based operating system **_iso_** image file.
  - Installation media or bootable device like, **_CD, DVD, USB Drive,_** etc.

### Overall installation process
Various linux based operating systems or distributions are available in the world like, **_Ubuntu, Fedora, Mint, CentOS, Red Hat_**, etc.
  - **_Step 1:_** Download the linux based OS **_iso_** image file of your choice or need of distribution and version.
  - **_Step 2:_** Create a bootable CD/DVD or USB drive.
  - **_Step 3:_** Boot that media on the target system, and then make a few decisions regarding the installation.

#### Build bootable device
First step is just download linux based OS from whatever site hosts the distribution and version you want. Once you've downloaded linux bsed OS **_iso_**, you'll need a utility that can create a bootable device. To achieve the target you can use **_rufus_**, which is fast, free and easy to use. To download **_rufus_** visit the site https://rufus.ie/en/ .
  - Plug in USB device and then run the **_rufus utility_**.

  ![Rufus Utility](../../images/installation/rufus-home-page.png)
  
  - In the **_Device_** field, at the very top, make sure your USB drive is the one selected. If not, click the pull-down and select it.
  - In the **_Boot selection_** field click on the **` SELECT `** option, navigate to the downloaded linux based OS folder and select the download **_iso_** image file.
  - If you like, you can change the **_Volume label_**, **_Partition scheme_**, **_Target system_** and other provided fields to your choice but it's not necessary. Click **` START `**, then wait while the drive is formatted and the **_iso_** copied or till **_Status_** is **` READY `**.

![Bootable Device](../../images/installation/making-bootable.png)

  - Click on **` CANCLE `** and exit.

#### Get ready to install
We will demonstrate with **_Red Hat Enterprise Linux_** installation. You can install any distribution with same approch but some internal steps will diifer. Now insert the bootable device into the target system or machine if wnat to install on **_Virtual Machine_** attach the **_iso_** image file under **_CD/DVD_** section in VM settings and power on the target machine. May need to change the **_boot order_**, in **_BIOS Setup_** or **_UEFI Firmware_** which by default almost certainly puts the hard drive first. 

To edit the **_BIOS Setup_** or **_UEFI Firmware_** setting press either **_F1_** or **_F10_** immediately after powering on the machine. It might also be either **_F2_** or **_F9_** or **_F12_** or even the **_Delete_** key depends on the system.

Select the desired option using **↑** and **↓** arrow, to move **_Up_** and **_Down_** use **+** and **-** key respectively and to expand press **_Enter_** key.

![BIOS setup](../../images/installation/bios-setup.png)

Once you've found your way into the **_BIOS Setup_**, find the **_boot_** or **_startup_** menu and make sure **_USB drive_** is first in the boot order. Then save and exit usually by pressing **_F10_** and accepting confirmation.

![BIOS setup Exit](../../images/installation/bios-setup-save-exit.png)

If everythis is as expected installation program **_anaconda_** will executed and you will land to the installation menu. You will see three options comming up here:
  - Install Red Hat Enterprise Linux *_<version_*
  - Test this media & install Red Hat Enterprise Linux *_<version>_*
  - Troubleshooting

![Installation Menu](../../images/installation/make-install-selection.png)

  - **_Install Red Hat Enterprise Linux 9.1_** will install the **_RHEL_** operating system using graphical installation program. <br>
  - **_Test this media & install Red Hat Enterprise Linux 9.1_** will first check the integrity of **_DVD_** or **_iso_** whether it's correct or not. If integrity will pass then proceed for installation. This will consume some time to test media. <br>
  - **_Troubleshooting_** contains several other options that you might be use to resolve system issue.

![Troubleshoot Options](../../images/installation/troubleshoot.png)

  - **_Install Red Hat Enterprise Linux 9.1 using text mode_** can be used if have trouble to installing **_RHEL_**. <br>
  - **_Rescue a Red Hat Enterprise Linux system_** if system having issue and not able to boot, make use of this option to access config files and edit them to recover system. <br>
  - **Run a memory test_** in case there is a fault in system's memory, can cause to system crash in that situation you can make use of this option.
  - **Boot from local drive_** boot from the local device drive which already have installed.

Since this is first time installation select the first option choose first option i.e. **_Install Red Hat Enterprise Linux 9.1_**. It will start the installtion process and will ask for **_language_** and **_region_** selection.

![Language Selection](../../images/installation/lang-selection.png)

Select the appropriate *_language_* and *_language country_* of your choice or requirement and click on **_Continue_**. At the next step you will see the **_Installation Summary_** where, you can configure everything according to your need. Such as **_Software Selection_**, **_Partition Creation_** and so on. <br>

![Installation Summary](../../images/installation/installation-summary.png)
  
To install Red Hat Enterprise Linux on local storage disk drive click on **_Installation Destination_** and make selection of your disk drive among all available and click on **_Done_** at top right corner.

![Installation Destination Selection](../../images/installation/install-destination-list.png)

Here you will see the all available storage disk drive attached to your machine and select your desired disk drive. you can select more than one if you wish or required. <br>
If you require to create partition on your own than select **_Custom_** and also you can encrypt drive to make it more secure. If you select the encryption option then you will ask for passpharase to encrypt partition and you will have to enter the same passpharase at every boot.

![Installation Destination List](../../images/installation/install-destination-selection.png)
  
Post disk drive selection and click on **_Done_** you will land to the partition creation page. You will see here three partition schemes: <br>
  - **Standard Partition**
  - **LVM**
  - **LVM Thin Provisioning**

![Partition Scheme](../../images/installation/partition-scheme.png)

**_Standard Partition:_** in this partition scheme we can't make changes without unmounting the partition. **_/boot_** partition must be standard. <br>
**_Logical Volume Manager (LVM):_** More flexible to scale up and scale down partition and can change on running machine without unmounting the partition. <br>
**_LVM Thin Provisioning:_** This allows to create logical volumes that are larger than the available extents.

![Partition Creation](../../images/installation/mount-point-selection.png)

To create partition or mount point click on **+** symbol and to delete partition click on **-** symbol. In linux generally you can create **_/boot_**, **_swap_** and **_/_** partition. 
  - Size of **_/boot_** may be **_512 byte_**.
  - **_swap_** you can create double of **_RAM_** size you have in system. If **_RAM_** size is greater than **_4GB_** than keep same as **RAM_** size but this all depends on your requirement and size available in your system.

    | **_Amount of RAM in the system_** |	**_Recommended swap space_**           | **_Recommended swap space if allowing for hibernation_** |
    |---------------------------------|--------------------------------------|---------------------------------------------------------|
    | Less than 2 GiB                 | 2 times the amount of RAM            | 3 times the amount of RAM                               |
    | 2 GiB - 8 GiB                   | Equal to the amount of RAM           | 2 times the amount of RAM                               |
    | 8 GiB - 64 GiB                  | 4 GiB to 0.5 times the amount of RAM | 1.5 times the amount of RAM                             |
    | More than 64 GiB                | Workload dependent (at least 4GiB)   | Hibernation not recommended                             |

  - The remaining space you can assign to the **_/ (root)_** partition. <br>
  - You may create more partition as per your need

Post creation all the required partition click on **_Done_** at top right corner and click on **_Accept Changes_**.

|                                                                            |                                                                 |
|----------------------------------------------------------------------------|-----------------------------------------------------------------|
| ![partition Selection](../../images/installation/partitation-creation.png) | ![accept Changes](../../images/installation/accept-changes.png) |

Now its time to select the required software to install at the time of installation itself. You can choose base environment either **_Server with GUI_** (graphical environment), or **_Minimal install_** (command-line environment) and some others. <br>
Additional softwares can be selected like, **_FTP Server_**, **_Mail Server_**, etc. You can install any software or package post installation as well at any time if you forgot any software at the time of installtion there is no worries. <br>
**_Note:_** <br>
All the settings depends on your requirement or choice.

![Software Selection](../../images/installation/software-selection.png)

Post selection of desired **_Base Environment_** and **_Additional software for Selected Environment_** just click on **` Done `** to apply the setting and return to the **` Installation Summary `**. 
  - Installation summary page provides all the settings which required such as **_Network & Host Name_**, **_Installation Destination_**, **_KDUMP_**, **_Time & Date_**, etc. 
  - **_kdump_** is a service which provides a crash dumping mechanism. The service enables you to save the contents of the system memory for analysis. 
  - In order to enable *_kdump_* a part of the system memory has to be permanently reserved for the capture kernel. 
  - **_Security Profile_** enables specific rules that cannot be applied later using remediation scripts, for example, a rule for password strength and partitioning. And has strict partitioning requirements that must be met, create separate partitions for **` /boot `**, **` /home `**, **` /var `**, **` /var/log `**, **` /var/tmp `**, and **` /var/log/audit `**.
  - Set the password for super user or administartor user which is, **` root `** password.

![Set root Password](../../images/installation/set-root-passwd.png)

If all the settings made properly or as per requirement then click on **_Begin Installation_** to start the installation process. Now sit back and wait to installation completion. Automatically it will install the operating system as per your settings.

![Begin Installation](../../images/installation/begin-installation.png)

Once the installation is completed it will prompt you to reboot the system. You will have to click on **` Reboot System `** and system will rebooted and everythins is as per expection then it will prompt you to login the system.

![Installation Complete Reboot](../../images/installation/installation-complete.png)

Now yor installation is done successfully and you can login with **_root_** user and password which, set by you at the time of installation phase.

![Installation Complete Reboot](../../images/installation/login-shell.png)

