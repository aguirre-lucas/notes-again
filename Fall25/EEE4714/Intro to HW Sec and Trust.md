# Overview of Course Content
* Cryptographic hardware vulnerabilities and processing overhead
* Attacks: physical, invasive, non-invasive/side-channel
* Physically unclonable functions (PUFs)
* True random number generation TRNG
* Anti-piracy: watermarking, passive and active hardware metering
* FPGA security: trusted design in FPGAs
* Hardware and Trojan detection and prevention
* Counterfeit detection and avoidance
# Goals
* Learning sota security primitives and methods as well as emerging technologies and security tends
* Integration of security as a design metric, not as an afterthought for the system
* Protection of the design intellectual property against piracy and tampering
* Better understanding of attacks and providing countermeasures against them
* Better understanding of vulnerabilities in design and fabrication processes
* Better understanding of the electronic component supply chain vulnerabilities
# What is Hardware?
* Electronic system
* System hardware - acts as the "root of trust": PCB --> IC (SoC | uP)
# Example Attack
* Roy Zoppoth stands over a Xerox 914 copy machine, the worlds first. IT was used in soviet embassies all over the world. It was so complex that the CIA used a tiny camera designed by  Zoppoth to capture documents copied on to the machine by the soviets and retrieved them using a "xerox repairman" in front of the soviets.
* HW Security is extremely necessary, the army and big companies require it embedded in their PCBs
* Pentagon's "Kill Switch"
	* Pentagon was worried about "backdoors" into their computers and it's various parts. These fears were confirmed by an Israeli attack on an alleged nuclear facility in Syria, where many speculate that Syrian air defenses had been sabotages by built in "kill switches" on commercial microprocessors in the Syrian radar that might've been purposely fabricated with that backdoor. By sending a preprogrammed code, it would turn off the chips functions and block the radar.
	* This was similar to the first Iraq war in America where the knockout blow was caused by a virus on a printer that disabled the entire Iraqi air defense. 
* DHS: Imported consumer tech contains hidden hacker attack tools
	* Top homeland securities admitted instances where along with software, the hardware components being imported from foreign parties and used in US systems are being compromised and altered to enable easier cyber-attacks
* The Hunt for Kill Switch, IEEE 2008
	* Increasing threat to hardware due to globalization
	* Extremely difficult to detect kill switches as well as intentional backdoors, which may have huge consequences
	* Example: the syrian radar's being jammed
* Fake Cisco routers risk "IT subversion"
	* An international FBI presentation states that counterfeit Cisco routers from China may cause unexpected failures in American networks. The equipment could also leave secure systems open to attack through hidden backdoors.
	* $76M to fake Cisco routers
* Energy Theft Going From Bad To Worse
	* Tampering with "smart" meters in Oil, electricity, and gas led to a $1B loss in CT because of electricity threat
* Medical Device Security
	* Incorporating security devices is sometimes considered expensive
	* Implantable devices like a heart rate monitor
		* Incorporating security could potentially reduce the life-time of the device by 30%
		* Attack these devices could cause loss of life
* Physical Attacks on Chip IDs
	* Extracting secret keys
* Side-Channel Attacks
	* Power Analysis, timing analysis, EM analysis
* Tampering with Electronic Devices
	* captured drone in iran
* Counterfeit ICs
	* multi-billion dollar business
	* 
# Motivation for Hardware Security
- HW Security is becoming increasingly more important
	- HW Security sneaks into PCs, according to CNET news.com
	- Microsoft reveals hardware security plans, concerns remail
	- Princeton professor finds no hardware security in e-voting machines
	- Secure chips for gadgets set to soar
	- Army requires security hardware for all PCs
# Rise of Clones
- Genuine vs Counterfeit products that contain hardware security vulnerabilities for consumers. 
# The landscape of Security in Modern Computing Systems
![[Pasted image 20251001130926.png]]
