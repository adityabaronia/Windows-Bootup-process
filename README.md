# Windows-Bootup-process
Windows booting process depends on whether the system is BIOS-based or EFI-based, *the bootup process differs up to the point of passing control to the kernel*.

## BIOS-Based system
On a BIOS-based system, the bootup process begins with the BIOS. The BIOS code selects a boot device and loads that device's Master Boot Record (MBR) into memory.
The MBR is 512 bytes in size and is located at the first sector of the device.It contains the boot code and the partition table. The partition table contains the location of the primary partition in the disk.
After the MBR is loaded, the BIOS passes control to the MBR boot code. The boot code parses the partition table and looks for a bootable partition. This is also called a system partition. After it is found, the MBR boot code reads the system partition's boot sector, which is found at the system partion's first sector.
The MBR then passes control to the boot sector code, which informs Windows on the nature of the volume and loads the Bootmgr file from the volume's root directory, after which, control is passed to the Bootmgr.
