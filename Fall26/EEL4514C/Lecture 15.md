# Agenda
1. Demodulation
	1. Power Efficiency of AM
	2. Demodulation of AM
# Demodulation
- If $\mu = 1$ the signal is 100% modulated
- If $\mu \gt 1$ the signal is over-modulated, we don't want this!!!!!
## Power Efficiency of AM
- Definition: $\eta = \frac{\text{useful power}}{\text{total power}}$
	- Let m(t) have bandwidth B, $f_c \gt B$ 
	- DC Component: $\bar{m}(t) = \int_{-\infty}^{\infty} m(t)dt = 0$ 
		- We can get rid of the dc component pretty easily with a capacitor
	- We're gonna call $||\cdot||^2$ as the symbol for power
		- $P_s = ||A\cos(\omega_ct) + m(t)\cos(\omega_ct)||^2$
		- ![[Pasted image 20261019012611.png]]power of cos() is A^2/2 and 1/2 when A=1![[Pasted image 20261019013014.png]]
		  integral of m(t) is zero based on the assumption that there's no DC component, but what is the second term? we don't know in the time domain, so we're gonna try in the frequency domain
		- ![[Pasted image 20261019013301.png]]Use the sampling property of delta(t)![[Pasted image 20261019013604.png]]![[Pasted image 20261019013733.png]]So while trying to compute the inner product, we see that it's all 0 :0. Then we see that $2<A\cos(\omega_ct), m(t)\cos(\omega_ct)> = 0$, so it's orthogonal
		- Now, let's compute $||m(t)\cos(\omega_ct)||^2$ in the freq domain
			- ![[Pasted image 20261019014036.png]]So this is the power of our signal!. We denote $P_m$ as the power of our original signal in the time domain
			- Total Power: $P_s = \frac{A^2}{2} + \frac{P_m}{2}$ ![[Pasted image 20261019014137.png]]
			- $\eta = \frac{P_m}{A^2+P_m}$
- e.g. tone modulation
	- $m(t) = \mu A\cos(\omega_m t)$
	- $P_m = \frac{\mu^2A^2}{2}$
	- $\eta = \frac{1}{2}\frac{\mu^2A^2}{A^2+\mu^2A^2} = \frac{\mu^2}{2+\mu^2} \cdot 100\%$ 
	- ![[Pasted image 20261019014453.png]]
		- since $\mu$ can't be greater than 1, this power efficiency fucking sucks
		- So AM is NOT power efficient, and wastes a lot of power, so why use it? The demodulation is *super* easy. 
## AM Demodulation
- Square demodulation
	- $s^2(t) = \left [ (A+m(t))\cos(\omega_c t)\right ]$
	- ![[Pasted image 20261019015007.png]]
	- Now just pass it through a LPF and you'll get the signal back!
	- ![[Pasted image 20261019015046.png]]This is the simplest demodulation of AM, but we don't use this in practice, why?
- ![[Pasted image 20261019015247.png]]
	- The bandwidth has doubled! Now the width of the "humps" in the freq domain are larger and require more strict rules on the carrier frequency where $2f_c-2B \gt 2B \rightarrow f_c \gt 2B$
	- ![[Pasted image 20261019015356.png]]