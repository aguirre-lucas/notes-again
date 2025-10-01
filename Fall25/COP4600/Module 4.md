# Process Scheduling
## Why do we need scheduling?
- Single CPU, single job
- CPU utilization in the figure: 9/14
- While doing IO the CPU is not used. 
- ![[Pasted image 20250930210647.png]]
- CPU utilization could be better if we had multiple programs/jobs
- Multiprogramming can increase CPU utilization
- ![[Pasted image 20250930210719.png]]
- In the best case here, the CPU is used 13/15 units of time
## Modeling
- Coarse Approach
	- We can do a very course measure of probability easily:
		- `p` Probability that one process is doing I/O (idle)
		- `n` Degree of multiprogramming (# of processes)
		- $p^n$ Idle probability
		- $1-p^n$ Utilization (not idle)
		  ![[Pasted image 20250930211018.png]]
		- 
		- ![[Pasted image 20250930211047.png]]
		- Takeaway: Big difference between the theoretical; and actual... Theoretical; approach not good for measuring scheduling efficiency...
## Schedulers: Three There Are
- Long-Term Scheduler: The Admission Scheduler - is there room?
	- The goal of this scheduler is to maximize throughput
	- Important for batch systems when there are jobs waiting
	- The question is: Is there room in the system for another job? (Good mix of jobs in RAM,CPU, etc.)
	- Generally, not used in Interactive OSs
	- Maybe for automated tasks such as backups when there is low load -- use long-term scheduler
	- ![[Pasted image 20250930211326.png]]
- Medium-Term Scheduler: The Memory Scheduler - is memory taxed?
	- Checks memory load, decides what remains in memory, what goes back to storage
	- Adds processes to memory or send them back to disk
- Short-Term Scheduler: The CPU Scheduler
	- Decides which process gets to be executed next in the CPU
	- Must execute fast! Really fast! Usually what we talk about when talking about schedulers
- ![[Pasted image 20250930212430.png]]
## Scheduling by OS Types and Goals
1. All Systems
	- Fairness: Giving each process a fair share of the CPU
	- Policy Enforcement: Seeing that stated policy is carried out
	- Balance: Keeping all parts of the system busy
2. Batch Systems
	- Throughput: maximize jobs per hour
	- Turnaround time: Minimize time between submission and termination
	- CPU utilization: Keep the CPU busy at all times
3. Interactive Systems
	- Response time; Respond to requests quickly
	- Proportionally: Mett users' expectations
4. Real-Time Systems
	 - Meeting deadlines: Avoid losing data
	 - Predictability: Avoid quality degradation in multimedia systems
 - Scheduling algorithms are also classified as either preemptive or non-preemptive
# Scheduling
## Batch System Scheduling
- Batch Schedulers:
	- maximize CPU utilization
	- Minimize mean turn-around time (MTT)
	- Can use priority-based scheduling
	- Can be preemptive or non-preemptive
	- Turn Around Time - Time from Finish to Arrival
	- Mean Turn ARoundTime (MTT) - Average TT (all jobs)
- Non-Preemptive Algorithms
	- Avoid most context switch costs and repsonse time from user interaction is not a priority
	- First-Cone First-Serve (FCFS): Jobs run in order of arrival
	- Shortest Job First (SJF): Job that will finish first will run first
	- Priority: Job with highest priority is executed first
	- Optimal (Crystal Ball): Uses future knowledge of arrivals for optimization (theoretical - base for comparison)
## Turn Around Time (TT)
- TT = finish - arrival
- ![[Pasted image 20250930233833.png]]
  - TT(A) = 9-0=9
  - TT(B) = 16-1=15
  - TT(C) = 12-2=10
  - MTT = (9+15+20)/3=11.33...
## First Come First Serve (FCFS)
- Non-Preemptive, basically a queue
- ![[Pasted image 20250930234048.png]]
- Doesn't give us the best results
## Shortest Job First
- Non-preemptive. Shortest at the time the current shortest is executing
- We need to know the time a job will take in advance
- ![[Pasted image 20250930234147.png]]
- If all jobs are ready, SJF is optimal. how can we know the "shortest" job?
	- Use history
	- Code analysis
	- Rule based approach
	- Basically, take estimates
## Priority
- non-preemptive
- ![[Pasted image 20250930234320.png]]
## Optimal Non-Preemptive: Crystal Ball
- Can't actually do this, it's a theoretical ceiling we can try to reach to achieve higher 
- With a crytal ball, the perfect MTT would be 9
# Preemptive and Real-Time Scheduling
## Preemptive Scheduling
- Has the ability to interrupt a currently working job to start another
- The scheduler sets a timer after it selects a process to run
- When the timer goes off, an interrupt is issued.
- The OS enters kernel mode; the scheduler is called
-  The scheduler selects another process
- Interactive Systems
	- Prioritize user responsiveness (sometimes at the cost of efficiency)
	- Almost always preemptive
- Algorithms
	- Shortest Remaining Time First (SRTF): Ready job that will finish first runs first
	- Round Robin (RR): Jobs run in order of arrival, with max quantum (end of queue if preempted)
	- Preemptive Priority: Ready job with highest priority is run first
	- Compatible Time-Sharing System (CTSS): Attempts to approximate SRTF
## Shortest Remaining Time First (SRTF)
- Preemptive
- ![[Pasted image 20251001001952.png]]
- Among preemptive algorithms, DRTF is optimal IF there's no context switch cost
## Round Robin
- Preemptive, basically a circular queue where each process runs for a some quanta of time
- ![[Pasted image 20251001002135.png]]
- How to determine the perfect RR quanta?:
	- We want a quanta that is long enough to minimize context switching, but short enough to give the user a good experience
## Multiple Queue Priority with RR(2)
- Multiple Queue: Usually exists in combination with other scheduling strategies, e.g. Roung Robin
- There is a queue for each priority
- ![[Pasted image 20251001002335.png]]
- Preemptive priority uses multiple queues and employs another scheduler, such as RR, on each
- ![[Pasted image 20251001003036.png]]
## Other Algorithms
- Proportionate Scheduling
	- Guaranteed Scheduling: If we have `n` processes, then we shouldn't ever allow any process to exceed `n` fraction of the total runtime. 
	- Lottery Scheduling: Each process gets "tickets", pick a ticket at random and more tickets means higher probability of being selected
	- Fair-Share Scheduling: CPU time allocated to user, not process, and can use lottery approach and prioritize users by number of tickets
## Compativle Time-Sharing Systems (CTSS)
- Algorithm that uses dynamic, growing quanta allocations and multiple priority queues
- Starts at Q0 quanta = $2^0$ =1
- Then Q1 quanta = $2^1$ = 2, then so on
- Queues with lower quanta have higher priority
- ![[Pasted image 20251001003558.png]]
## Real-Time Priority
- Static Schedule Priorities
	- Priority of tasks don't change
	- Runs highest priority task first
	- Is preemptive
- Dynamic Schedule Priorities
	- Prioritizes scheduling according to the system's deadlines
	- Hard Real Time: missed deadline = disaster (flight control fail)
	- Soft Real Time: annoying (video is choppy)