# Agenda
1. SSB Modulation
2. VSB
3. QAM
- All of these^ are under bandwidth efficient modulation
# SSB Modulation
- Consider the USB signal in the frequency domain $$S_{USB}(f) =M_+(f-f_c) + M_-(f+f_c)$$Where $M_+(f) = \frac12[ M(f) + jM_h(f)]$ and $M_-(f) = \frac12[ M(f) - jM_h(f)]$
- ![[Pasted image 20261020184841.png]]So, $S_{USB}(t) = m(t)\cos(\omega_c t) - m_h(t)\sin(\omega_c t)$
	- The first part of the equation is the DSB-SC and the second part is ALSO a DSB-SC on a sin() carrier, but it doesn't send m(t) but instead the Hilbert Transform of m(t)
- ![[Pasted image 20261020185220.png]]
- So now, how can we do the demodulation? Since we know the squaring and rectifying process is just for the AM, let's try to just multiply it by a cos(), aka the coherent demodulation
- $e(t) = s_{USB}(t)\cos(\omega_c t)$
	- ![[Pasted image 20261020185532.png]]It works! Using coherent demodulation we can retrieve the original signal and remove the other parts with a LPF
- There's a slight issue though with the USB. The sharp discontinuity of the beginning of the sidebands would be unrealistic to create in real life, so we come up with something to mitigate this by creating a modulation approach that solves this.
# Vestigial Sideband (VSB) Modulation
- Vestigal: remnant, something left behind. 
- We're gonna make the transition smoother![[Pasted image 20261020193727.png]]
	- Only downside is we need a little more bandwidth, but not as bad as DSB
- ![[Pasted image 20261020193929.png]]
	- we have input/output filters to create the smooth transition shapes in the frequency domain that are then transmitted and filtered out
	- note we demodulate with 2cos(omega_ct)
- $S_{VSB}(f) = M(f-f_c) + M(f+f_c)H_i(f)$ 
	- ![[Pasted image 20261020195148.png]]
	- Before we do this, let's see the idea for the demodulation. Ideally, we want a perfect filter to remove the lower half. But what if our filter isn't perfect? Our resulting signal will look like this, and we can't recover our exact m(t)![[Pasted image 20261020204028.png]]
	- Ideally, $M(f) = \left [ S_{VSB}(f-f_c) + S_{VSB}(f+f_c) \right ]H_o(f)$
		- $= M(f) \left [ H_i(f-f_c) + H_i(f+f_c) \right ]H_o(f)$
		- Making sure the H_i and H_o terms multiplied is equal to 1
		- so $\rightarrow H_0(f) = \frac{1}{H_i(f+f_c) + H_i(f-f_c)}$
# Quadrature Amplitude Modulation (QAM)
- Widely used in digital communications
- We tried saving bandwidth by cutting the signal in frequency by half and making the transition be less harsh. We cut off the signal in frequency with the Hilbert function, but now let's transmit an independent signal instead. With QAM we don't shrink the bandwidth and instead transmit the original modulated message with another new message. Also, the filtering for SSB is complex
- Motivation: 
	- We want better bandwidth efficiency than DSB-SC/AM
	- Don't want the complexity of SSB/VSB
- Idea: Like SSB modulation, use two carries at the same frequency with a 90 degree offset (quadrature). 
- ![[Pasted image 20261020210658.png]]It's just like ssb but instead of a hilbert transform signal it's an independent signal
- $S_{QAM}(t) = m_1(t)\cos(\omega_c t) + m_2(t)\sin(\omega_c t)$
