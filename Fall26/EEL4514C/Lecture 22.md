# Agenda
1. Demodulation (FM)
	-  Phase Locked-Loop (PLL)
# Demodulation (FM)
2. Phase Locked-Loop
- $s(t) = A\cos[\omega_c t + \theta_i(t)]$ where $\theta_i(t) = k_f \int_0^t m(\alpha)d\alpha$
- The goal is to find out what the phase is![[Pasted image 20261110135323.png]]![[Pasted image 20261110135514.png]]So now the output of VCO is $B\cos(\omega_c t + \theta_o(t))$  
	- Now this e(t) formula is very nice because e(t) is proportional to the derivative of $\theta_o(t)$, which means that in our assumption that $\theta_i(t)$ and $\theta_o(t)$ are roughly the same, $e(t) \approx \frac{k_f}{k_v} \theta_i'(t)$, which is our input phase, and the derivate of the input phase is our message!
	- Therefore $e(t) \propto m(t)$ !!![[Pasted image 20261110135934.png]]
- PLL detailed analysis (for creating the loop filter)
	- Equivalently, this is our PLL, now let's use the laplace transform to analyze this![[Pasted image 20261110140555.png]]So here, we can see the output phase is a function of the input phase
	- Now, let's look at this for the error E(s)![[Pasted image 20261110140825.png]]So now we have the error in laplace domain as E(s) by replacing $\hat{\theta}(s)$ and doing that algebra, and now modeling $\theta(t)$ as a function of time with respect to a fixed frequency slope and fixed phase offset, giving us a laplace domain representation.
	- Proportional Control
		- Let F(s) = 1. This obviously is not a low pass filter, like how we expect the loop filter to behave, so![[Pasted image 20261110142744.png]]using Final Value Theorem![[Pasted image 20261110142901.png]]So if we have only the phase error, F(s) =1 works, but if we have a fixed frequency offset, then this doesn't work. 
			- Proportional control works if there is only a phase offset, no frequency offset. 
		- Let $F(s) = k_1 + \frac{k_2}{s}$, aka a Proportional + Integrator (PI)
			- ![[Pasted image 20261110143229.png]]So if you do the math that Ji won't, it works! It's probably in the book but i don't need the math to believe it, i kinda see it. 