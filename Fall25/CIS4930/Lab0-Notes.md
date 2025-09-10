These will be the notes of all the stuff to be learned from the HTB Academy Linux Fundamentals Module. 
# Linux Structure
- An operating system, OS, is software that manages all the hardware resources of a computer, facilitating communication between software applications and hardware components. Linux comes in many "distros". 
- History:
	- Unix released by Ken Thompson and Dennis Ritchie (who both worked for AT&T) in 1970. 
	- The Berkely Software Distribution (BSD) was released in 1977, but since it contained the Unix code, AT&T sued the shit out of them. 
	- Richard Stallman started the GNU project in 1983, to create free Unix-like operating systems.  His work helped created the General Public License (GPL). Projects over the years failed to result in a working, free kernel that would become widely adopted until the creation of the Linux kernel. 
	- Started as a personal project by Linus Torvalds, to create a new, free operating system. Over the years, went from a small number of files in C to to over 23 million source code lines, licensed under GNU GPL. 
	- Available in over 600 distributions, e.g., Ubuntu, Debian, Fedora, OpenSUSE, elementary, Manjaro, Gentoo Linux, RedHat, and Mint. 
	- Considered more secure than other operating systems. Less susceptible to malware than Windows. Difficult for beginners and does not have as many hardware drivers. 
	- Free and open source, and the source code and be modified and distributed commercially or non-commercially, and linux-based OS exist everywhere in IoT. Android OS is based on linux kernel, hence why Linux is the considered the most downloaded OS. 
	- Linux is an OS line Windows, iOS, Android, or macOS, and is a software that manages all the hardware resources associated with our computer. Manages whole communication system between software and hardware. 
	- Pwnbox is a fork of Parrot OS
# Philosophy
- Everything is a file
- Small, single purpose programs
- Ability to chain programs together to perform complex tasks
- Avoid captive user interfaces
- Configuration data stored in a text file
# Components
- Bootloader - piece of code that runs the guide to the booting process to start the OS. 
- OS Kernel - main component of OS. Manages resources for system's I/O devices at hardware level.
- Daemons - background services are called "daemons" and they ensure that key functions are working correctly. These small programs load after we boot
- OS Shell - shell or command language interpreter (command line) that interfaces between the OS and the user. Bas, Tcsh/Csh, Ksh, Zsh, Fish
- Graphics server - provides a graphical sub-system (server) called "X" or "X-server" that allows graphical programs to run locally or remotely on the X-windowing system
- Window manager - GUI. GNOME, KDE, MATE, Unity, Cinnamon. Desktop environment 
- Utilities - programs that perform particular functions for the user or another program
# Linux Architecture
- Hardware - peripheral devices like RAM, HDD, CPU, etc
- Kernel - core of the Linux OS whose function is to virtualize and control common computer hardware resources like CPU, allocated memory, accessed data, and others. Gives each process it's own virtual resources and prevents/mitigates conflicts between processes.
- Shell - CLI that user can enter commands to
- System Utility - Makes available to the user all of the operating system's functionality
# File System Heirarchy
- `/` Root - top level directory
	- /bin - contains essential command binaries
	- /boot - consists of static bootloader, kernel executable, and files required to boot the OS
	- /dev - Contains device files to facilitate access to every hardware device attached to the system
	- /etc - Load system config files and app config files
	- /lib - shred library files that are required for system boot
	- /media - external removable media devices such as USB drives are mounted here
	- /mnt - temporary mount point for regular filesystems
	- /opt - Optional files such as third-party tools here
	- /home - each user on the system has a subdirectory here for storage
	- /var - contains variable data files such as log files, email in-boxes, web app cookies, cron files, more
	- /usr - contains executables, libraries, man files, etc
	- /tmp - everything stores temp files here, cleaned on system boot and deleted at other times without warning
	- /sys - weren't talked abt
	- /proc - 
	- /root - 
	- /run - 
# Linux Distributions
- Exist to do different things, like run servers or interact with embedded devices to casual usage to mobile devices. Tailored for specific markets. Popular distros include:
	- Ubuntu
	- Fedora
	- CentOS
	- Debian
	- Red Hat Enterprise Linux
- Most people choose linux cuz its free and open source and customizable. We choose it cuz it's code is available for scrutiny and customization.
- Main differences between distros are the packages , the UI, and tools available. Kali Linux is the most popular for cyber security specialists. Ubuntu is widespread for desktop users, and Debian is popular for servers and embedded systems. Rat Hat and CentOS are popular for enterprise-level computing. 
  Debian:
	- Widely used and well respected It uses `apt` Advanced Package Tool package management system to handle software updates and security patches. This is fire and can upgrade manually or automatically. 
	- Have a steep learning curve, but regarded as the most flexible and fun.
	- Biggest strength is long-term support releases, and always having security patches. Especially important for up and running systems that use Debian. 
# Intro to Shell
- Crucial.
- Shell is a text-based GUI in which we enter commands to perform actions like navigating to other directories, working with files, and obtaining information from the system but with way more capabilities.  
# Terminal Emulators
- software that emulates the function of a terminal. Allows the use of text-based programs within a GUI. There are also CLIs that run as additional terminals in one terminal. In short, a terminal serves as an interface to the shell interpreter. 
- Terminal emulation software acts like a virtual receptionist's desk to the server room, without having to be there. 
- CLIs run additional terminals within one terminal are like having multiple receptionists open on your screen simultaneously. Allowing you to send different instructions at the same time, independently. 
- Terminal emulators and multiplexers are beneficial extensions. Use TMUX!
# Shell
- Most commonly used shell is BASH. It's awesome, but others exist, like mac uses Zsh. 
# Prompt Description
- Bash is simple to understand.
```bash
<username>@<hostname><current working directory>$
```
- Home is denoted with a `~`
```bash
<username>@<hostname>[~]$
```
- The dollar sign, in this case, stands for a user. As soon as we log in as `root`, the character changes to a hash `#` and looks like
```bash
root@htb[/htb]#
```
- If we don't see anything, the PS1 variable in the environment may not be set correctly. in this case, we'd at least see the Unprivileged or Privileged shell prompts,
```bash
$ #unprivileged
```
```shell
# #privileged 
```
- The PS1 variable is how you control how your terminal *looks.* Customizing PS1 lets you change display info. It displays basic info like username, computer name, current directory, and even add colors and special characters. 
- You can also customize it to show useful details like IP address, date/time, succes/failure of last command.
- List of special characters you can use in your PS1 variable:![[Pasted image 20250910011933.png]]
- Customizing the prompt is one step in making your terminal experience more efficient and personalized. 
# Getting Help
- We will always see tools we don't know what to do with. We can get familiar with them by their `man` pages and their help functions.
- `man <tool>` to get it's page. Details everything about a tool, including flags and arguments.
- some tools like `ls` use `--help` as an argument as well to get info, others like `curl` use `-h`
- For short explanations `apropros <keyword>`  is your go to:![[Pasted image 20250910012252.png]]
- If all else fails: https://explainshell.com/
- 