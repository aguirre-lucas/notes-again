# SoC Introduction
1.    SoC  - Trends: Moore's Law
	- IC transistor capacity doubles every 18 months forever
- Definitions
	- System: A collection of all kinds of components and/or subsystems that are appropriately interconnected to perform the specified functions for end users. 
	- System-on-Chip: Complete system on a single chip?
	- SoC not only chip, but more "system"
		- SoC = Chip + Software + **Integration**
- SoC Hardware includes
	- Embedded processor
	- Hardware IPs
	- Peripherals and analog circuitry
	- Embedded memory
	- Interconnect
- SoC Software includes
	- OS, compiler, simulator, firmware, driver, protocol stack, IDE
2. SoC - Applications 
	- Speech Signal Processing
	- Image and Video Signal Processing
	- PC interface, computer peripherals
	- Data communication (ethernet, xDSL) 
	- Wireless communication (bluetooth, wlan, 2G/3G/4G/5G, UWB)
	- Consumer
		- Playstation/Xbox
		- Digital Camera
		- Cars
- Benefits
	- Reduced size
	- Reduced overall system cost
	- Lower power consumption
	- Increased performance
	- Increased functionality/performance in reduced footprint
	- Simplified PCB design
	- Increased product mechanical robustness
3. SoC - Challenges 
	- Increasing complexity
		- TTM pressure
		- Verification bottleneck
	- Integration
		- HW vs SW
		- Digital circuits vs Analog circuits
		- Testing Issues
	- Deep submicron effects
		- Timing closure problem
		- Signal integrity problem
		- Reliability problem
	- Manufacturing costs
- Time-to-Market Pressure
	- Shorter product lifespan
	- Shrinking design cycles
	- ![[Pasted image 20260116013337.png]]
- Design Productivity Gap
	- While designer productivity has grown at an impressive rate over the past decades, the rate of improvement has not kept pace with chip capacity
	- ![[Pasted image 20260116013712.png]]
- The Mythical man-month
	- The situation is even worse than the productivity gap indicates
		- In theory, adding designers to team reduces project completion
		- In reality, productivity per designer decreases due to complexities of team management and communication
	- In the software community, known as the "mythical man-month"
	- At some point, can actually lengthen project completion time! (too many cooks)
	- ![[Pasted image 20260116013951.png]]
- Verification Bottleneck
	- Verification becomes the major bottleneck of the modern design flows
		- From 30-40% to 50-80%
	- ![[Pasted image 20260116014434.png]]
	- An effective verification methodology is also highly desirable
	  ![[Pasted image 20260116014449.png]]
- HW/SW Integration
	- Integrating HW/SW at the final step may require high cost to fix inconsistency problems
	- ![[Pasted image 20260116014600.png]]
- Challenges for Mixed Signal Designs
	- Design challenges:
		- Analog, mixed signal blocks
		- PLL circuit, Pad circuits
		- Chip-level simulation is time consuming
		- low-level iterations consumes plenty of time
		- Design is not completely systemic
		- There is limited or no use of HDL (VHDL/Verilog AMS but not a standard yet)
- SoC Testing Challenges
	- Distributed design and test
		- Core provider doesn't know the target environment
		- System integrator is responsible for manufacturing testing
	- Test access
		- Difficulties to access deeply embedded cores
		- Bandwidth, I/O pin count limitations
	- Test optimization
		- Minimizing test cost while satisfying constraints such as power, resources, coverage, etc.
- Nanometer Design Challnges
	- In 2005, feature size was 0.1um, uP frequency = 3.5 GHz, die size =520 mm2, uP transistor count per chip = 200M, wiring level = 8 layers, supply voltage = 1 V, power consumption = 160 W
	- Feature size down, noise? wiring? reliability?
	- Frequency up, dimension up, interconnect delay? emg field effects? timing closure?
	- Chip complexity up, large-scale system design methodology?
	- Supply voltage down, signal integrity?
	- Wiring level up, manufacturability? 3D layout?
	- Power consumption up, power & thermal issues?
	- ![[Pasted image 20260116015554.png]]
- Timing Closure Problem
	- Wire delay starts to dominate total delay in DSM process
		- Cannot be ignored as taught in DD
	- Only statistical wire delay model can be used at design phase
		- Lack of physical information about wire length
	- Incorrect estimations require long iterations to meeting timing
	- Design schedule will be seriously delayed !!
	- ![[Pasted image 20260116015940.png]]
- The need for a new level of abstraction
	- RTL well understood, but...
		- IP reuse difficult
		- Architectural decision not possible until after RTL verification
		- Level of complexity no more possible at RTL-level
	- Transaction-Level Modeling (TLM)-IP as golden source allows for
		- Design reuse
		- Reduce verification time
		- Fewer bugs
		- Hardware/software co-verification
		- Function calls instead of cycle accurate
- RTL Limitations
	- ![[Pasted image 20260206181632.png]]
-  TLM as "Golden Source": Expectations
	- Faster IP Created and Design Reuse
	- Increase verification throughput
	- HW/SW co-verification
	- Combine RTL/TLM Verification
	- Reusing Verification IP from TLM through RTL
	- Incremental design refinement
- ![[Pasted image 20260206184600.png]]
- ![[Pasted image 20260206184804.png]]
- Tools used in class
	- OpenCV
	- UVm
	- SystemC
	- VHDL/SystemVerilog
	- Spartan7
# Components Intro
- SoC Components
	- An SoC usually consists of
		- Processor, DSP, FPGA
		- Embedded memory
		- Custom Hardware
		- Peripherals
		- Mixed Analog SIgnals
		- Interconnection
		- (RT)OS and embedded software
- Intro and Motivation
	- Embedded systems are at the heart of critical systems and infrastructure
	- Design Objectives:
		- Construct an implementation with desired functionality
		- SoC-Centric
- Architecture Synthesis - Problems
	- Parallel Programs Problem: Simultaneous task-mapping and selection of resources
	- Processor made up of microprocessors
	- Communication networks can be either grid, link, or bus
	- More challenging: simultaneously optimize numerous design metrics
		- Unit cost, NRE
		- Size, Power, Weight
		- Performance
		- Flexibility, maintainability
		- Time-to-market, time-to-prototype
		- Resilience, Security
	- Detailed understanding of SoC components is paramount
		- Processors
			- Level of parallelism: ILP, TLP, numerous design metrics
			- Interface: Memory, Bus, etc
			- Memory: Register File, cache levels
			- Accelerator: Vector
		- Interconnect
			- Topology, scalability
			- Throughput, latency
			- Size, Power, Weight
		- Memory
			- Size, latency
			- Associativity
			- Single or Multiported
	- ![[Pasted image 20260206193026.png]]
# CPU (1)
- Processor technology
	- Used to implement a system's desired functionality
	- Processor does not have to be programmable. "Processor" != general-purpose processor
	- ![[Pasted image 20260206195317.png]]
	- Processors vary in their customization for the problem at hand
	- ![[Pasted image 20260206195911.png]]
- General-Purpose Processors
	- Programmable device used in a variety of apps
		- Also known as uP
	- Features
		- Program memory
		- General datapath with large register file and general ALU
	- User benefit
		- Low TTM and NRE costs
		- High flexibility
	- Pentium the most well-known, but hundreds of others
	- ![[Pasted image 20260206200241.png]]
- Single-purpose processors
	- Digital circuit designed to execute exactly **one** program
		- aka coprocessor, accelerator or peripheral
	- Features
		- Contains only the components needed to execute a single program
		- No program memory
	- Benefits
		- Fast
		- Low power
		- Small size
	- 
	- ![[Pasted image 20260206200352.png]]
- Application-specific processors
	- Programmable processor optimized fora particular class of applications having common characteristics
		- Compromise between general-purpose and single-purpose processors
	- Features
		- Program memory
		- Optimized datapath
		- Special functional units
	- Benefits
		- Some flexibility, good performance, size and power
	- ![[Pasted image 20260206200453.png]]
- General Purpose Processors (CPU)
	- ![[Pasted image 20260206201519.png]]
	- Instruction processing style
		1. Instruction fetching
		2. Instruction decode
		3. Operands fetching (memory or registers)
		4. Execute
		5. Write back result (memory or registers)
		6. Define next instruction fetch
		- Repeat
	- ![[Pasted image 20260206201628.png]]
	- ![[Pasted image 20260206201637.png]]
- Instruction-Level Parallelism
	- Pipelining!
	- Ann, Brian, Cathy, and Dave each have one load of clothes to wash, dry, and fold
	- Washer takes 30 minutes
	- Dryer takes 40 minutes
	- Folding takes 20 minutes
	- ![[Pasted image 20260206202934.png]]
	- ![[Pasted image 20260206202946.png]]
	- Pipelining doesn't help **latency** of single task, it helps **throughput** of entire workload
	- Pipeline rate limited by **slowest** pipeline stage
	- Multiple tasks operating simultaneously
	- Potential speedup = Number pipe stages
	- Unbalanced lengths of pipe stages reduces speedup
	- Time to "fill" pipeline and time to "drain" it reduces speedup
- Pipelining the MIPS-DLX
	- ![[Pasted image 20260206203122.png]]
	- ![[Pasted image 20260206235007.png]]
- Speedup
	- Performance: # of Instructions per cycle (IPC)
		- $IPC_{no-pipe} = 1$
	- Speedup with Pipelining
		- $IPC_{pipe} = k$
		- ![[Pasted image 20260206235108.png]]
- Pipelining is not quite that easy!
	- Limits to pipelining: **Hazards** prevent next instructions from executing during its designated clock cycle
	- Types of hazards are:
		- Structural hazards: HW cannot support this combo of instructions
		- Data hazards: Instruction depends on result of prior instruction still in the pipeline
		- Control hazards: caused by delay between the fetching of instructions and decisions about changes in control flow (branches and jumps)
- When you only have 1 memory port, a structural hazard can appear like so![[Pasted image 20260206235300.png]]
- You would resolve the hazard by stalling one cycle ![[Pasted image 20260206235445.png]]
- Speedup Equation for Pipelining
	- $CPI_{pipelined} = \text{Ideal CPI} + \text{Average Stall cycles per Inst}$
	- $\text{Speedup} = \frac{\text{Ideal CPI} \times \text{Pipeline depth}}{\text{Ideal CPI} + \text{Pipeline stall CPI}} \times \frac{\text{Cycle Time}_{unpipelined}}{\text{Cycle Time}_{pipelined}}$
- For a simple RISC pipeline, CPI=1
	- $\text{Speedup} = \frac{\text{Pipeline depth}}{1 + \text{Pipeline stall CPI}} \times \frac{\text{Cycle Time}_{unpipelined}}{\text{Cycle Time}_{pipelined}}$


2. Example: Dual Port vs Single port
	- Machine A: Dual-ported memory ("Harvard Architecture")
	- Machine B: Single ported, but pipeline implementation has a 1.05x faster clock rate
	- Ideal CPI = 1 for both machines
	- Loads are 40% of instructions executed
		- Calculate the speedups to find which machine is better
		- ![[Pasted image 20260218173814.png]]
	- Machine A is 1.33 times faster
- Data hazard on R1
	- ![[Pasted image 20260218174124.png]]
	-  We have to wait for r1 to be calculated before it can be used in any other instructions. 
	- ![[Pasted image 20260218174222.png]]
	- We can use data forwarding to prevent data hazards. Take the result from the ALU and keep the pipelining going
	- Doesn't always work!
	- ![[Pasted image 20260218174306.png]]
	- We can edit our architecture so forwarding is possible at all stages ![[Pasted image 20260218174340.png]]
	- There's still data hazards that forwarding unfortunately cannot solve ![[Pasted image 20260218174516.png]]
	- The data in the register after reading from memory won't be available to forward to the ALUs. For this, we add a "bubble" or stall ![[Pasted image 20260218174600.png]]
- Control Hazards on Branches
	- ![[Pasted image 20260218174628.png]]
	-  If we have an instruction like beq and then some instruction after that depends on the branch, what do we do to fix this hazard?
	- We would have to stall the pipeline in order for this branch to execute properly, in this instance three times
	- If CPI = 1, 30% branching
		- Stall 3 cycles => new CPI = 1 + 0.30(3) = 1.9, nearly doubling our cpi!
	- Two-part solution:
		- Determine if branch taken or not sooner
		- Compute branch target address earlier
	- DLX branch tests if register = 0 or $\ne$ 0
	- DLX Solution:
		- Move zero test to ID/RF stage
		- Adder to calculate new PC in ID/RF stage
		- 1 clock cycle penalty for branch versus 3!!
	- ![[Pasted image 20260218175857.png]]
	- Alternative solutions include:
		- Stalling until branch direction is clear (we saw this is bad)
		- Predict Branch Not Taken
			- Execute successor instructions in sequence
			- "Squash" instructions in pipeline if branch actually taken
			- Advantage of late pipeline state update
			- 47% DLX branches not taken on average
			- PC + 4 already calculated it, so use it to get next instruction
		- Predict Branch Taken
			- 53% DLX branches taken on average
			- But haven't calculated branch target address in DLX
				- DLX still incurs 1 cycle branch penalty
				- Other machines: branch target known before outcome
		- Delayed Branch (compilers usually do this)
			- Define branch to take place AFTER a following instruction
```asm
branch instruction:
	sequential successor1
	sequential successor2
	...
	sequential successor_n
branch target if taken:
	here!
``` 
/
			*  1 slot delay allows proper decision and branch target address in 5-stage pipeline
			* DLX uses this
		
* Code that compiler can write for \[\[likely]] and \[\[not likely]] flags and stuff![[Pasted image 20260218183959.png]]
* Where do we get instructions to fill branch delay slot?
	* Before the branch instruction
	* From the target address: only valuable when branch taken
	* From fall through: only valuable when branch not taken
	* Canceling branches allow more slots to be filled
* Compiler effectiveness for single branch delay slot:
	* Fills about 60% of branch delay slots
	* About 80% of instructions executed in branch delay slots useful in computation
	* About 50% (60% * 80%) of slots usefully filled
* Delayed Branch downside: 7-8 stage pipelines, multiple instructions issued per clock (superscalar)
- Evaluating Branch Alternatives
	- $\text{Pipeline speedup} = \frac{\text{Pipeline Depth}}{1 + \text{Branch Frequency} \times \text{Branch Penalty}}$
	- ![[Pasted image 20260218185836.png]]
- Most processors today have 8-16 pipeline stages
- If more pipelines equal a larger speedup, why not have 1 morbillion pipeines?
	- More pipelines means more instructions in architecture, which means more hazards to deal with
- If we can increase pipeline, where can we get that improved performance of the processor?
	- We can make instructions longer
	- CISC architectures
- If we look at say, Intel's CISC processors, they usually break down long instructions to become RISC
	- A controller inside intel's CPUs takes in variable-length instructions and converts it to RISC-type instructions
- If we extend the processor stage, can we include multiple ALUs to promote parallelism? Yes!
- Multiple Issue Processors
	- Parallelism always gives us the performance boost we're looking for, pipelining is a form of parallelism
	- Can we duplicate that and have multiple instructions in this block, and multiple instructions in another? Yes! This is known as multiple issue processors
	- Super pipelining:
		- very deep pipeline, ex. Pentium 4: 20 stages
	- Multiple issues:
		- Several function units with corresponding datapaths
			- CPI < 1
			- IPC = 1 / CPI, >1 here
		- Static Issue
			- VLIW-Processors
			- Compiler is in charge of determining the instruction needed to fit all units in the architecture and tell those units what to do at the same time
		- Dynamic Issue
			- Superscalar Processors
			- Hardware does the VLIW stuff dynamically
3. VLIW Processors
	- Package multiple operations into one instructions
	- Example:
		- One integer instruction
		- Two independent floating-point operations
		- Two independent memory references
	- Must be enough parallelism in code to fill the available slots
	- 