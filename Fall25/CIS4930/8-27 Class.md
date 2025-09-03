## Class on thursday's is in 1084

# Intro to Networking
* What is a network?
	* Computers connected communicating to each other
* Network Devices
	* Anything
		* Computers, phones, routers, switches, etc
		* Contain at least one **Network Interface Card** (NIC)
			* Wired or wireless connection to the internet
		* ![[Pasted image 20250827140535.png]]
# Basic Topology
* ![[Pasted image 20250903011108.png]]
* Router and Switches
	* At home, switches are combined in to the router
	* Switches allow for communication to the router
	* Going from one network to the internet, you need a router
	* Switch = MAC Address, Router = IP Address
	* Every device on a network has it's own IP address, sometimes more than 1 for routers
	* Internal vs External IP cards within a router. Routers have both. 
* get other slide from powerpoint with more complex topology
* Firewall
	* Usually integrated within a router, but can be separate.
* ![[Pasted image 20250903011133.png]]
# Lingo
* IP Address
* Subnet Mask
* Router
* Default Gateway/Router (kinda)
* Service
* Protocol
* Interface
* Firewall
* Port - NOT physical but digital. 25526 for minecraft. {IPv4}:25526. 80(unencrypted) or 443(encrypted) for Web Traffic
# Ports
* Numbers that identify, along with an IP address, which network socket to connect to on a given device
* Common port numbers and associated services:
	* TCP 20 and 21 - FTP
	* TCP 22 - SSH
	* TCP 25 - SMTP
	* UDP 53 - DNS
	* TCP 80 - HTTP (internet unencrypted)
	* TCP 443 - HTTPS (internet encrypted)
# Subnet Masks
* ![[Pasted image 20250827142806.png]]
* Refers to which part of the IP address refers to the network and which part refers to the devices
* Ex: CISE website:
* ![[Pasted image 20250827142941.png]]
* 128.277 identifies the network. 19.86 defines the devices. Aligns with the 255.255.0.0
* Think in binary: $$1111\space 1111. 1111\space 1111.0.0$$ This means that the first two will always stay the same, but the bottom two can change dependent on the device
# Linux
* What is it?
	* NOT an operating system
		* Distributions or flavors of linux
	* Free and open source kernel (now referred to as an operating system)
	* Built on Unix (unix-like)
* Hardware->Kernel->Shell->Applications 
* ![[Pasted image 20250903011323.png]]
* Terminals
	* bash, zsh, terminal emulators 
	* "Desktop" Version (big interface)
	* "Server" Version (no interface just black screen and command line)
* File Tree - Contents of /
	* boot, etc, proc, dev, home, root, var, usr, run, tmp, mnt, opt
	* usr/bin, usr/sbin
	* [put photo here]
* On linux, **everything** is a file
* Paths and Directories
	* Absolute path starts with /
	* Relative path starts with pwd
	* Current directory is .
	* Previous directory is ..
	* cd /home/user/Desktop (absolute)
	* cd /var/www/html (absolute)
	* /etc/ssh/sshd_config (absolute)
	* cd .. (previous)
	* ./script.sh (relative and current)
	* var/www/html (relative)
* root vs /root vs /
	* root = 
	* /root = directory
	* / = prefix of directories
* Text Editors
	* `nano` - simple, goated
	* `vim` - harder but more functionality
* Shell and Syntac
	* *command* -**options** arguments
	* Eg:
		* ls
		* cd /home/user1
		* ls -la user1/Downloads
		* ls -R
* Viewing Files
	* ls (list) --> ls -flags filepath
	* -l (more detailed)
	* -a (show all files and directories)
* Creating Files
	* touch --> touch filename
	* touch filepath/filename
	* nano/vim/nvim/etc filename
* Common commands 
	* cp (copy) - cp (source)
	* mv (move) - mv (source)
	* cat (concatenate) - cat (filename) 
	* rm (remove) - rm (flags) (filepath/filename)
	* mkdir (make directory) - mkdir (flags) (path/director_name)
		* -p or --parent as flags for mkdir
* Linux File Permissions
	* 