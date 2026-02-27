# Chapter 1
- Typical hardware for DSP implementation includes:
	- CPU/uC
	- Digital Signal Processor
	- GPU
	- FPGA
	- ASIC
- decreasing order in _programmability/reconfigurability_ and an increasing order in _speed_.
## Hardware Platform
- RealDigital RFSoC 4x2 board![[Pasted image 20260227030540.png]]
	- 4 RF inputs and 2 RF outputs![[Pasted image 20260227030622.png]]
	- PS
		- 64-bit quad core ARM APU
		- 32-bit dual core ARM RPU
		- ARM Mai-400 GPU
		- 8-channel DMA
		- PCIe, SATA, DisplayPort, Gbit Ethernet, USB3
		- Platform Management Unit and Configuration Security Unit
	- PL
		- 425k CLB LUTs
		- 850k FFs
		- 4k DSPs
		- 38Mb BRAM in 2080 blocks, 22.5Mb URAM in 80 blocks
		- Sixteen 33Gbps GTY transceivers
		- IP for PCIe and Ethernet
		- GPIO
	- RF System
		- 8 14-bit RF ADCs supporting up to 5 Gsps sampling rate
		- 8 14-bit RF DACs supporting 9.85 Gsps sampling rate
		- 8 integrated SD-FEC blocks
		- 8 PLLs
	- ![[Pasted image 20260227031012.png]]
	- AXI4 interconnects
## Software Development Tools
- We use:
	- 1 ADC, corresponding PLL, and data converter in the RF system
	- DSP kernels implemented using resources in the PL
	- ARM Cortex A53 APU in the PS as host control of DSP, interfacing with global memory, with an embedded linux OS running on the APU
	- Control signals interfacing between PS, PL, and RF systems via AXI4
- Vitis-Vivado Toolchain
	- PL
		- Configures ADC, PLL. Converts data to provide a stream of input signal samples at rate and interface with ADC sample stream to the DSP kernels in the PL. Handled in RTL
		- Implements DSP kernels, each with interface to ADC, other DSPs, and global memory, handled in HLS with C/C++
	- PS
		- Develops drivers and APIs to control PL and transfer data between host and DSP kernels
		- Develops application program on host to control and communicate with the DSP kernels and to present the processed results from the DSP kernels
	- Vivado
		- Does the RTL of the stuff for the PL
	- Vitis HLS
		- Supports the HLS design of DSP kernels, can be synthesized and used in Vivado for FPGA implementation
	- Vitis
		- Supports host application/driver development and build
		- Invokes Vitis HLS and Vivado to perform PL design and implementation
		- Packages host OS and bitstream for running embedded DSP application on the board
## VAAD
- Vitis Application Acceleration Development (Flow)
- ![[Pasted image 20260227031738.png]]
- ![[Pasted image 20260227033313.png]]
- Class Platform ![[Pasted image 20260227033321.png]]
# Intro to HLS
- Macro-architecture --> Micro-architecture using HLS
## HLS Tasks
- HLS code is untimed, so we don't program with the notion of the clock. 
- RTL specification of a DSP kernel is timed however, but the HLS tool will handle this. 
- Typical digital architecture for the DSP kernel includes:
	- datapath
	- control logic
	- memory, AXI4 interface, other
- Main functionality of the DSP kernel is implemented in the datapath, which includes:
	- storage elements (registers, memories)
	- function units (multipliers, ALU)
	- interconnect elements (mux, tri-state drivers, bus)
- HLS tool needs to  perform these tasks in order to produce the RTL specification of the DSP kernel from HLS
	- Compilation
		- g++, compiles and optimizes code, however not in a standard way.
	- Scheduling
		- HLS tool determines which operations occur in which cycles, and amount of PL resources needed to support this
	- Binding
		- HLS tool assigns specific PL resources to implement each scheduled operation as well as memories and registers required. Resource sharing is implemented, along with the RTL implementation of scheduled operations. 
	- Control Logic Extraction
		- Creates FSM
	- External Interface Generator
		- Title, makes interface for the DSP kernel
	- RTL Generation
		- Title
## Scheduling & Binding Example
```c++
int ex1(char x, char a, char b, char c) { 
  int y; 
  y = x*a+b+c; 
  return y; 
}
```
- `x` and `y` are the input and output of the kernel and `a`, `b`, and `c` are kernel parameters
- This image depicts how the assignments are produced by the HLS tool in scheduling and binding![[Pasted image 20260227034039.png]]
- Scheduling determines:
	- Can be completed in two clock cycles
	- Schedule `x*a+b` in the first cycle and store in register 
	- Take that result and add `c` in the second
- Binding determines
	- Initial binding assigns a combinatorial multiplier and an addsub components to each of the two additions
	- Target binding decides to use a DSP slice since the Mul and AddSub can be implemented in one DSP slice
## Control Logic Extraction Example
```c++
 void ex2(int in[3], char a, char b, char c, int out[3]) {
   int x, y;
   for (int i = 0; i < 3; i++) {
     x = in[i];
     y = a*x+b+c;
     out[i] = y;
   }
}
```
-  The FSM and datapath the HLS tool will generate will look like this![[Pasted image 20260227035824.png]]
- ![[Pasted image 20260227035842.png]]
## Vitis HLS Development Flow
- Labs
- C/C++ Kernel Coding
	- Write the HLS
- C-Simulation
	- Verify the functionality of the kernel code with a C++ testbench
- Code Analysis
	- Analyze the performance, parallelism, and legality of the kernel code (linter)
- C-Synthesis
	- Generate the RTL specs for given clock and i/o constraints with `v++`
- C/RTL Co-Simulation
	- Verify the RTL code using the C++ testbench and review the HLS synthesis and implementation timing reports. 
- Package
	- Generate DSP kernel object `.xo` for use in the VAAD flow
- ![[Pasted image 20260227040326.png]]
# Design Basics
- Write C++, but don't actually write C++. You'll see
- We write code that will be
	- Synthesizable
	- Follows good practices
## Performance Metrics
- Throughput
	- If implementing an FIR/IIR filter, the input and output are both streams of signal samples, and hence the throughput of the kernel is the rate of the output sample stream
	- If implementing FFT, the input is a stream of signal samples and the output is a stream of FFT blocks, hence the throughput is the amount of FFT blocks per second calculated and outputted
- Interested in amount of CLBs, DSP slices, power, and RAM consumed by DSP kernel. Always try to minimize
- Pipelining! Essential
- Iteration latency
	- How long it takes for the first input item to enter the pipeline and produce the first (valid) output item. Iteration latency specifies a transient characteristic of the pipeline
- Initiation Interval (II) is the amount of time required to elapse between successive operations by the same station. Basically, how long it takes for successive outputs to be produced by the pipeline.
- II=1 Is the expectation!! We try our hardest to get this
## Producer-Consumer Model
- Typically, a signal source like an ADC is providing a stream of samples that is fed into a DSP task for processing. The DSP task may produce an output stream of samples that is fed into another task, so on and so forth, eventually reaching a signal *sink* (PS). 
- This config is a producer-consumer model in which a DSP task acts as a **producer** of samples and another dsp task downstream acts as a **consumer**. This allows us to represent the kernel as a dataflow graph.
- We should write our C++ to follow this producer-consumer model, while vitis can pipeline and parallelize at the task level
## Pipelining
- Tasks done simultaneously to speedup
- ![[Pasted image 20260227044035.png]]
## Parallelization
- Independent tasks done in parallel
- ![[Pasted image 20260227044050.png]]
## FIFO and PIPO
- For the producer-consumer model to work, we need tasks to be synchronized. Handshaking has to take place so we know when data is produced and when data is consumed. Synchronization quickly becomes more complex the more tasks exist
- Standard solution is to use a streaming buffer between producer and consumer, which allows data to be freely pushed and read. Synchronization implicitly achieved!
- **Care must be taken in the design process such that the insertion rate of data by the producer into the streaming buffer cannot be higher than the retrieving rate of data by the consumer; otherwise, the buffer length will need to grow indefinitely to avoid dropping data or deadlock!!!!!**
- Fifo
	- Buffer queue with pre-determined depth in which elements must be received (popped) sequentially from the queue in the order it came in (first in first out)
- Pipo
	- Two buffers used to hold producer's old output data for the consumer to consume while producer inserts new data. The roles of the two buffers switch after the consumer finishes consuming the data in the old buffer and producer completes inserting data into the new buffer.
- Comparison
	- PIPO requires more PL
	- PIPO has random access
	- Data in a PIPO buffer can't be consumed until producer finished filling it up, unlike FIFO. 
	- PIPO allows simultaneous access, while FIFO must take turns inserting/removing
	- FIFO of insufficient depth can cause deadlock, PIPO ensures deadlock free
## Deadlock
- OS, resources infinitely waiting for each other to finish, or all are sharing the same definite resource. 
- ![[Pasted image 20260227044721.png]]
- Read this if you're bored:![[Pasted image 20260227044743.png]]
## Task Model
- ![[Pasted image 20260227045818.png]]
- Basically gonna be creating this as much as possible. Sacrilege to do anything else i think
- Execution Unit
	- Performs DSP function of task
- Local Memory
	- BRAM, URAM, Registers depending on implementation. Stores data accessible only within the task
- Ports
	- Parameters, return values, superior memory and such
- In order for us to design this unit in C++ we have to
	- Have the execution unit specified by the operations within the body of the function
	- Variables in the body of the function mapped to local memory, Vitis handles which type (registers/RAM for scalar arrays typically)
	- Arguments of the task function are mapped to the I/O ports. Input ports for scalar arguments are connected to registers and ports for arrays and streams are connected to streaming buffers. The directions of the ports are inferred from read/write operations on the streaming buffers defined in the body of the function. 
- Vitis supports blocking and non-blocking read/write semantics for accessing the connected streaming buffers in a task. A blocking read from an empty buffer holds the reading process until the buffer becomes non-empty. A non-blocking read from an empty buffer returns uninitialized or the last read data. Similarly, a blocking write to a full buffer holds the writing process until the buffer becomes not full. A non-blocking write to a full buffer results in the data being dropped.
## Data-driven Execution Model
- We won't be doing this
- Not controlled by any actions like function calls and data transfer by the PS, just executed when there's data to produce/consume and such
- Here's an example tho:
```c++
void splitter(hls::stream<int>& in, hls::stream<int>& odds_buf,
              hls::stream<int>& evens_buf) {
int data = in.read();
if (data % 2 == 0))
  evens_buf.write(data);
else
  odds_buf.write(data);
}

void odds(hls::stream<int>& in, hls::stream<int>& out) {
  out.write(in.read() + 1);
}

void evens(hls::stream<int>& in, hls::stream<int>& out) {
  out.write(in.read() + 2);
}

void odds_and_evens(hls::stream<int>& in, hls::stream<int>& out1,
                    hls::stream<int>& out2) {
hls_thread_local hls::stream<int, N / 2> s1; // channel connecting t1 and t2
hls_thread_local hls::stream<int, N / 2> s2; // channel connecting t1 and t3

// t1 infinitely runs func1, with input in and outputs s1 and s2
hls_thread_local hls::task t1(splitter, in, s1, s2);

// t2 infinitely runs func2, with input s1 and output out1
hls_thread_local hls::task t2(odds, s1, out1);

// t3 infinitely runs func3, with input s2 and output out2
hls_thread_local hls::task t3(evens, s2, out2);
}
```
![[Pasted image 20260227050334.png]]
## Control-driven Execution Model
- The normal one we will be doing
- Kernel controlled by PS host through function calling and parameter passing, and can access global memory. 
- We can write regular C++ to define the kernel and use `#pragma HLS dataflow` the region of code we want Vitis HLS to infer and construct an efficient dataflow graph with pipelining of. It will also apply task-level parallelization too
- Dataflow region's canonical form:
	- Task functions are not inlined
	- Each task function's return type is void
	- Each task function uses only local and non-static variables
	- The sequence of task functions pass data forward such that an acyclic dataflow graph can be inferred. C++ scalar/array arguments can be employed to pass data from one task function to the next. Arrays will be mapped to buffers. If cyclic data flow is required, feedback connections must be use `hls::stream` or `hls::stream_of_blocks` arguments
	- Array argument variables linking a producer task to a consumer task are written before read
	- No conditional, loop, return, goto, exception are used to control data flow in the sequence of task functions
- Example
```c++
typedef unsigned char data_t;

void diamond(data_t I[N], data_t O[N]) {
  data_t c1[N], c2[N], c3[N], c4[N];
#pragma HLS dataflow
  A(I, c1, c2);
  B(c1, c3);
  C(c2, c4);
  D(c3, c4, O);
}

void A(data_t* in, data_t* out1, data_t* out2) {
#pragma HLS inline off
Loop0:
  for (int i = 0; i < N; i++) {
#pragma HLS pipeline
    data_t t = in[i] * 3;
    out1[i] = t;
    out2[i] = t;
  }
}

void B(data_t* in, data_t* out) {
#pragma HLS inline off
Loop0:
  for (int i = 0; i < N; i++) {
#pragma HLS pipeline
    out[i] = in[i] + 25;
  }
}

void C(data_t* in, data_t* out) {
#pragma HLS inline off
Loop0:
  for (data_t i = 0; i < N; i++) {
#pragma HLS pipeline
    out[i] = in[i] * 2;
  }
}

void D(data_t* in1, data_t* in2, data_t* out) {
#pragma HLS inline off
Loop0:
  for (int i = 0; i < N; i++) {
#pragma HLS pipeline
    out[i] = in1[i] + in2[i] * 2;
  }
}
```
- Not even gonna touch the mixed version
# HLS Programming
- The good coding practices
