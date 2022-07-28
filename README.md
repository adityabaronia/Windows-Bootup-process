# Windows-Bootup-process
Windows booting process depends on whether the system is BIOS-based or EFI-based, *the bootup process differs up to the point of passing control to the kernel*.

## BIOS-Based system
On a BIOS-based system, the bootup process begins with the BIOS. The BIOS code selects a boot device and loads that device's Master Boot Record (MBR) into memory.
The MBR is 512 bytes in size and is located at the first sector of the device.It contains the boot code and the partition table. The partition table contains the location of the primary partition in the disk.
After the MBR is loaded, the BIOS passes control to the MBR boot code. The boot code parses the partition table and looks for a bootable partition. This is also called a system partition. After it is found, the MBR boot code reads the system partition's boot sector, which is found at the system partion's first sector.
The MBR then passes control to the boot sector code, which informs Windows on the nature of the volume and loads the Bootmgr file from the volume's root directory, after which, control is passed to the Bootmgr.
Bootmgr operate in real mode. This means that what's on disk is what's in memory. There is no virtual to physical translation of memory. But the first this does is switch the operational mode of the CPU to protected mode. As a result, the full 32bits of memory become accessible, enabling the bootmgr to access not just the 1 MB(20bits) of physical memory, a limitation in real mode, but all of them. For Windows to operate normally, the system must be running in protected mode with pageing enable. So after switching to protected mode, the bootmgr also enables paging.

**NOTE**
For BIOS-based systems, the Bootmgr briefly switches back to real mode to execute BIOS functions, especially if Bootmgr needs to access the computer display and integrated development emvironment.

Once the system is running in protected mode with paging enable, the bootmgr then loads the Boot Configuration Data (BCD) store in the boot folder located on the root directory of the system volume.
As defined by Microsoft, the BCD store contains boot configuration paramenter and conterol how the operating system is started. Once the BCD is loaded, it directs the Bootmgr to the partition where the Windows is located. This is also known as the boot partition of the boot volume. From here, the bootmgr lpoads Winload.exe, which is the Windows boot loader.

**NOTE**
The BCD can have multiple boot-selection entries that can include other operating systems. If this is the case, the Bootmgr displays the OS choices to the user and the user decides which one to boot. If the user does not chhose anything with the given time frame, Bootmgr loads the bootloader of the default OS.


Noe that control os passed to the Windows OS loader, Winload.exe, it gathers hardware description about the system and then loads the files needed to initialize the kernel. It loads Ntoskrnl.exe and Hal.dll and their dependencies. Winload.exe then reasds the SYSTEM registry hive loacated in \Windows\System32\Config\System.The registery hive constains the boot device drivers that must be loaded to boot the system. The regstry path that contains the subkey of the boot device drivers is HKLM\SYSTEM\CurrentControlSet\Services. It contains not just the subkeys of the boot device dreivers, but subkeys of all the device drivers.
