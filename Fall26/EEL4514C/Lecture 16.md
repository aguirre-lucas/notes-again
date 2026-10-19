# Agenda
1. Demodulation of AM
2. Bandwidth Efficient Amplitude Modulation
# Demodulation of AM
- Three approaches
	- Single Sideband (SSB)
	- Vestigal Sideband (VSB)
	- Quadrature Amplitude Modulation (QAM)
- Rectifier/Diode/Envelope Demodulator
	- ![[Pasted image 20261019141029.png]]
	- So we need to get the analog portion here so, $$R(t) = \left [ A + m(t)\right ]\cos(\omega_ct) \cdot w(t)$$Where w(t) is defined as the rectangle wave
		- ![[Pasted image 20261019141828.png]]
		  The width of the square waves is $\frac{1}{2f_c}$ 
		- Let's do a fourier series representation
		- $w(t) = \frac12 + \frac{2}{\pi} \left [  \cos(\omega_ct) - \frac13\cos(3\omega_ct) + \frac15 \dots\right ]$
			- ^^ this is the fourier series expansion (2.8 in 5th edition book)
		- $r(t) = s(t)w(t)$
		- ![[Pasted image 20261019142221.png]]
		- Ignore the higher frequency components since it'll be attenuated by a LPF and then DC remover and such
		- Diagram: ![[Pasted image 20261019143215.png]]
			- coherent demodulation: need to know the exact phase or delay in time in order to demodulate the signal. this is what we needed before
			- now in order to try to solve that problem, we tried to send the carrier with it. in this case, there might be ways to take it out but we didn't actually extract m(t) explicitly, we did another approach that didn't rely on knowing the phase, this is a non-coherent demodulator. 
			- so, these are non-coherent receivers since they don't generate the local carrier.
			- coherent is preferred, and apparently there's always a way to get it but we'll find out later
- Bandwidth Efficient Demodulation
	- Single Sideband Modulation (SSB)
		- Consider a DSB signal m(t) that's real (symmetric in freq, $M(-f) = M^*(f)$)
		  ![[Pasted image 20261019144031.png]]
		  if we modulate we get
		  ![[Pasted image 20261019144200.png]]
			- Why not just send the USB and LSB only? why send both? let's try it
		- ![[Pasted image 20261019145547.png]]
			- top is USB modulation and bottom is LSB modulation. Let's see how it works
		- ![[Pasted image 20261019145918.png]]
		  we can't get what we want just by multiplying a cos. we want a flip in the negative side and a regular shift in the positive side. 
		- If we know M(f), how do we get just the positive part? We just multiply it by the step function, or specifically the sgn(f) function where  $sgn(f) = \begin{cases} 1, \qquad f \gt 0 \\ -1, \qquad f \lt 0 \end{cases}$ 
		- ![[Pasted image 20261019150311.png]]
			- $M_+(f)= \frac12M(f) [1 + sgn(f)] = M(f)u(f)$ aka the step function :0 (1+sgn(f) is u(t))
		- In the freq domain this is happening, in the time domain we're convolving. First, let's look at the ift of sgn(f) $$\mathcal{F}^{-1}\{sgn(f)\} = \frac{j}{\pi t}$$
		- so now $$m(t) = \mathcal{F}^{-1}\{M_+(f)\} = \frac12 \left [ m(t) + jm_h(t) + \frac{1}{\pi t} \right ]$$
		- This is the Hilbert Transform
			- the hilbert transform of a real signal m(t) is $m_h(t) = m(t) * \frac{1}{\pi t}$
		- Let $h(t) = \frac{1}{\pi*t}$ , then $H(f) = \mathcal{F}\{-jsgn(f)\}$
		- ![[Pasted image 20261019150942.png]]
		- Let's look at some properties of H(f), which means H(f) is -j for f > 0, and j for f < 0
		  ![[Pasted image 20261019151212.png]]
			- So this filter does not change the magnitude, but it shifts the phase by a factor of pi/2
		- So let's see how to generate the signal![[Pasted image 20261019151627.png]]
		- Now we have both the USB and LSB parts 
		- USB: $M_+(f) = \frac12 [M(f) + jM_h(f)] = M(f)u(f)$
		- LSB: $M_-(f) = \frac12 [M(f) - jM_h(f)] = M(f)u(-f)$
		- 