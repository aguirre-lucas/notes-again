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
* get from powerpoint later
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
* Terminals
	* bash, zsh, terminal emulators 
	* "Desktop" Version (big interface)
	* "Server" Version (no interface just black screen and command line)
* File Tree - Contents of /
	* boot, etc, proc, dev, home, root, var, usr, run, tmp, mnt, opt
	* usr/bin, usr/sbin
	* [put photo here]
* On linux, **everything** is a file
* root vs /root vs /
	* root = 
	* /root = directory
	* / = prefix of directories
* Text Editors
	* `nano` - simple, goated
	* `vim` - harder but more functionality
* Linux File Permissions
	* 