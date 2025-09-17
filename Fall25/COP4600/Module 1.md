# Intro to Operating Systems
## Why do we need an OS?
- How to load a program onto a computer?
	- Mini toggle switch per bit in the data register
	- Mini-toggle switch per bit in the address register
	- Button to load data from register into memory
- How to access I/O Devices?
	- Uniform interface: use the same read/write routines
	- Safety: restrict access to read/write routines
- What about running multiple programs?
	- Manage resources
	- Protect private info
	- Facilitate interactions
- ![[Pasted image 20250917183935.png]]
## What is the OS's Job?
1. Resource manager
2. Extended machine
## OS as a Resource Manager
- Process Manager
	- Next program to be executed? Time to be given to each program
- Memory manager
	- Best use of memory to run as many programs as possible
- I/O Device
	- Which program should a particular I/O device use
## OS as an Extended Machine
- ![[Pasted image 20250917184251.png]]
	- (i would never call the frog ugly) 
## What the OS is Not
- User Interface
- System Tools
- Libraries
## Classifications of OS?
- Interactive OSs - prioritize user response time
- Batch OSs - run large sets of tasks or jobs
- Real Time OSs - have well defined task priorities
## Gen 1 OS:
- Vacuum Tubes and Plugboards
- Programmer / user = operator
- Single program at a time
## Gen 2 OS:
- Transistors and Batch Systems
- Users were able to execute a sequence of programs called "batches"
- OS was necessary, although rudementary
- The role of the "programmer" became well defined, the user was NOT the operator
- Tape storage
- 2A: uniprogrammed. 2B: multiprogrammed. almost pipelining
## Gen 3:
- ICs and Multiprogramming
- Smaller and better, more reliable
- Networks started to exist, where things can communicate through a terminal
## Gen 4 and 5:
- currently in it
# Named Calls
## File Storage
- Operating systems facilitate storage of data via File Systems
- File Systems:
	- Pattern to the arrangement of data on disk
	- Have a mounting scheme :C, /mnt/c
	- Usually involve a naming and directory system
- Other possible features
	- redundancy
	- networking
## Programs & Processes & Threads, Oh My!
- When we run a program (list of instructions) we create a process for it
- A process:
	- is a running program (in execution)
	- has some address space
	- is associated with specific resources
	- Has one or more threads of execution
	- ![[Pasted image 20250917192006.png]]
## Named Calls
1. Procedure Calls
2. System Calls
## Procedure Calls
- Procedures are named code with:
	- Local variables
	- Parameters
	- Pointer to entry/return
- Here, function = procedure
	- Typically, a function does not change input parameters, being a special case of procedures
### Process Memory Layout
![[Pasted image 20250917192203.png]]
## System Calls
- involves a context switch into kernel mode for execution
## System Calls and System Library
- User program calls System Library (similar to a procedure call)
- Inside the System Library Routines, the System Call is actually made. Parameters and System call numbers are stored by the System Library Routine at specific spaces
- TRAP to Kernel, aka a context switch from user mode to kernel mode
- Kernel dispatcher reads a SysCall number, looks up a vector table, and makes an internal call to code that carries out the syscall
- System Returns to Library
- Library routine returns to user space
- ![[Pasted image 20250917192613.png]]
- ![[Pasted image 20250917194640.png]]
# Interrupts
micro peenar
## Hardware Interrupts
- interrupts the flow of execution of the software, and is controlled separately from the CPU
- ![[Pasted image 20250917194844.png]]
- Steps:
	- Some process requests a file and is blocked
	- I/O device reads the file and is telling the system that it is ready by issuing an interrupt
	- Current instruction gets interrupted, and may potentially switch to kernel mode
	- Dispatcher finds the correct handler, and based on the IQR# by running the interrupt service routine ISR, may need dat and stack to run the ISR
	- Interrupt handler executes
	- When done, returns control to user mode
## Interrupted Interrupt?
- Interrupt mask - part of the interrupt controller architecture. it is a binary value stored on a register
- Masks implement protection states, aka who can interrupt you?
- ignored interrupts typically just stau raised
- Previous interrupts must be resorted when the interrupt is complete:
	- Remember old mask values and restore them
	- most architectures have a dedicated register for this!
## Fetch -- Decode -- Execute
![[Pasted image 20250917195215.png]]
## Precise vs Imprecise Interrupts
- Precise interrupts:
	- PC is saved in a known state
	- All instructions preceding PC were fully executed
	- No instructions after the PC have been executed
	- Execution state of PC instruction is known
- Imprecise interrupts
	- If instructions are left incomplete/partially executed, it results in an imprecise interrupt
	- Modern processors have precise interrupts
## System Calls vs Interrupts
![[Pasted image 20250917195402.png]]
