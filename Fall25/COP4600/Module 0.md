# Module 0 Lecture Video
In this course, we're looking at how an operating system manages inputs and outputs properly.
A typical computer architecture will include:
* CPU
* Memory
* Video Controller
* USB controller
* Fixed Storage Controller
These will all be connected by a bus. The bus will define the flow of data and connect these components together
## Inside the CPU
* Registers
	* CPU Memory
	* Speical & General Purpose
* ALU
	* Arithmetic computations
	* Logic computations
* Control Unit
	* Tracks state / status
	* Controls other components
* Fetch/Decode/Excecute
	* Load instruction (fetch)
	* Find opcode / operands. interpret instruction (decode)
	* Carry out instruction (execute)
	* Protected instructions can only be executed in a protected state
* Pipelining
	* ![[Pasted image 20250902221147.png]]
	* Lets CPU perform multiple instructions at once. 
	* ![[Pasted image 20250902221218.png]]
	* Can have buffers and other things to make it run smoothly.
* Access times for an 8-core Skylake Processor, as an example:

| Type      | Time      | Amount |
| --------- | --------- | ------ |
| Registers | 0.33ns    | 1KiB   |
| L1 Cache  | 1ns       | 512KiB |
| L2 Cache  | 4ns       | 2MiB   |
| L3 Cache  | 12ns      | 16MiB  |
| SDRAM     | 50ns      | 16GiB  |
| SSD       | 100$\mu$s | 1TiB   |
| HDD       | 10ms      | 8TiB   |
* Access time is longer the further away from the CPU you are, but so does the size of the memory. 
* Memory Size Units:
* ![[Pasted image 20250902221556.png]]
## The Boot Process
* Starting your computer!
* intel i386 boot process is the example we will be used here.
* BIOS = Basic Input Output System
* I386 Boot Process:
	1. Execute BIOS Init (drivers, etc.)
		1. Stored in a ROM chip, and was manufactured by many companies but doesn't rly have a standard
		2. Checks if basic components like display and keyboard are detected, and if so will run POST
	2. Run Power-On Self Test (POST)
		1. If something is wrong, the computer will beep, and problems can be diagnosed by the pattern of the beeps.
		2.  If POST is successful, will find boot device in the non-volatile RAM (NVRAM)
	3. Get boot device via NVRAM (no standard)
		1. most BIOS information had to be entered into the BIOS directly due to the lack of standardization, info including the boot device.
		2. Once the boot device was loaded, the BIOS reads the first sector, "boot sector",  of that boot device.
	4. Load "Boot Sector" into volatile RAM
		1. Main information in the boot sector is the boot block program, which contains the address of the function that initializes the OS 
	5. Execute Boot Block program from the Boot Sector (one block)
		1. Once executed, the boot block program bootstraps (or executes)  by calling the bootloader 
	6. Bootstrap OS with Bootloader routine (flexible)
		1. Each OS has their own bootloader program. It can be anywhere in the disc and have any size
![[Pasted image 20250902223357.png]]
- But what if there are multiple operating systems inside one system?
	- Assuming a single disk drive with multiple partitions, logically, the system will see each partition as it's own logical disk. 
	- The boot sector is now the Master Boot Record, which will hold a menu asking the user which OS to bootstrap in to.
	- Once an OS is selected, the first sector of that OS will be loaded in to memory and the boot process will continue as before, but with the specified boot process for that OS. 
- ![[Pasted image 20250902223420.png]]
## X64 Boot Process - UEFI
* Currently found in most modern systems, replaced the BIOS with the UEFI (Unified Extensible Firmware Interface Standard). **Standard** for the implementation of boot process sytems!
* This allows the operating system to assume where basic information about the system can be stored. This allows the operating system to manage boot disks and other low-level system configurations. A consequence of this is that users can change those settings from within the operating system without having to interact with the UEFI during the boot process. 
* Typically found within the X64 system, similar to x86 but for 64-bit systems
* Boot Process:
	1. Execute architecture firmware
		1.  performs an initial verification and initialization process of the computer's hardware. Checks for basic components like display and keyboard
	2. Execute EFI Manager (init & drivers)
		1. EFI looks for the boot device location where the OS can be found
	3. Get boot device via NVRAM (standardized)
	4. On Disks, get boot loader from EFI partition and the OS is called
![[Pasted image 20250902224214.png]]
* Protective Master Boot Record - keeps message that says that the disk contains an UEFI and may contain an operating system. This is a preventative measure so that if an UEFI disk is inserted into a disk running BIOS, users will be warned not to format the disk imagining that it is empty or corrupt. 
* Primary Header - Initial information about the disk stored here. Capacity, sector size, number of sectors, basic info about the partitions in the disk. UEFI disks have several partitions even if they host only one OS
* Partition Entities - Stores metadata about the existing partitions. Labels, sizes, structure, any information
* EFI Partition (green) - Contains bootloaders for the two OS in red and green. WIth the bootloaders in the same partition as it's safer from attacks. Bootloaders do the same thing as they did in BIOS, initialize the operating systems. 
* Partition Entries 2 - Safety measure if the first one is corrupted or lost, so if anything happens it can be restored.
* Secondary Header - Safety measure that copies the first one in case of anything
## ARM Boot Process
* ARM (like many platforms) has no standardized boot process
* Advanced RISC Machine. RISC is typically faster and simpler than CISC constructions. RISC vs CISC
# C Language by Dr Nemo