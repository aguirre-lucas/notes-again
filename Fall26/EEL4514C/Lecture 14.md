# Agenda
1. Demodulation
# Demodulation
- Get M(f) from S(f) or R(f) *(typically denote the received signal as r(t)*
- $$s(t) = m(t)\cos(2\pi f_c t)$$If we want to retrieve m(t), let's look at the freq domain![[Pasted image 20261018225734.png]]
  We want to shift our spectra back to the middle somehow someway, in the time domain this is a phase shift.
- Let's try $$s(t)\cos(2\pi f_c t)$$ where $$\begin{align} s(t) &= m(t)cos(2\pi f_c t) \\ &= m(t)\cos^2(2\pi f_c t) \\ &= m(t) \left ( \frac12 + \frac12\cos(4\pi f_c t) \right ) \\ &= \frac12m(t) +  \frac{1}{2}m(t)cos(4\pi f_c t) \end{align}$$Where 1/2m(t) is the baseband signal and almost what we want and the second term is just the modulated signal at 2fc.
- If we do a FT where above is e(t) $$E(t) = \frac12M(f) + \frac14M(f-2f_c) + \frac14M(f+2f_c)$$![[Pasted image 20261018230504.png]]
- So basically to demodulated the simplest modulating signal, we just multiply by another cos() and then pass it through a LPF. 
- Consider $r(t) = s(t-\tau)$ where $\tau$ is the propagation delay
	- Why does this matter? The phase is delayed so it could mess with amplitudes, but why would that matter in amplitude modulation? 
	- $$r(t) = m(t-\tau)\cos(2\pi f_c t- 2\pi f_c \tau), \text{ let's call the second term } \theta$$
	- e.g. $\theta = \frac{\pi}{2}$ ![[Pasted image 20261018235055.png]]Here there's no baseband signal left, so if our receive signal is delayed we can't recover it, only the high frequency part. We fail to demodulate
	- Now let's try $\theta = \pi$![[Pasted image 20261019000420.png]]Every single bit will be inverted and incorrect. If we know the phase we can correct it, but obviously we won't. So after the LPF, every amplitude is negated. 
	- So, unless we know the delay, we cannot use this approach directly
- So there's two ways to solve this
	- 1. Just send the carrier together with the DSB-SC signal --> AM Radio
		- $s_{AM}(t) = m(t)\cos(2\pi f_c t) + A\cos(2\pi f_c t)$
		- ![[Pasted image 20261019002825.png]]
		- Although it's no longer SC, at the receiver side if we just take out the pure carrier part, we can multiply the signal of the received signal and since they'll both be out of phase we can
	- 2. Use a control loop to match the phase of the local carrier to the phase of the receive signal --> (Phase Lock Loop, PLL)
- So let's do a FT of $s_{AM}$
	- ![[Pasted image 20261019003240.png]]
- e.g. Let's look at a random m(t)![[Pasted image 20261019003406.png]]
	- 2 cases: m(t) + A > 0, for all t![[Pasted image 20261019003427.png]]
	- So this is easy to demodulate with an envelope detector circuit that we'll learn about later that can get this envelope out. So we don't need to multiply by anything we can just do analog detection
	- What if m(t) + A < 0, for some t![[Pasted image 20261019003627.png]]
	- So we can't do an envelope detector here since there's a phase inversion. 
	- The condition for envelope detection of AM is $A + m(t) \gt 0 \qquad \forall t$ 
		- $-m_p \le m(t) \le m_p, \qquad A \ge m_p$ basically saying what value of A needs to be for condition to be true
- A is useful for demodulation, but it does not carry any new information and just wastes energy. In order to see that, we introduce the Modulation Index
	- For an AM signal, the MI (Modulation Index) $$\mu = \frac{m_p}{A} \le 1$$ where $m_p$ is the maximum absolute amplitude of the message signal 