# Agenda
1. QAM
2. Applications of Amplitude Modulation
3. Angle Modulation 
# QAM
- $S_{QAM}(t) = m_1(t)\cos(\omega_c t) + m_2(t)\sin(\omega_c t)$
	- Doing the inner product of the first and second term returns 0! Which means it's orthogonal![[Pasted image 20261020220458.png]]
	- Due to orthogonality, we can imagine our baseband signal as $m_1(t) + jm_2(t)$
	- So in analysis, we use ^ but in practice we use the first $S_{QAM}(t)$ definition. We should always imagine signals as complex signals
	- In all analysis, we only assume complex signal, because before we had 2 signals but now we have 1. It's also easier to analyze because if we modulate by cos we have 2 signals, but with the complex signal it's no longer symmetric. The complex modulation uses one complex exponential, so using that we don't have a negative frequency component anymore, which is easier to analyze!
	- ![[Pasted image 20261020221724.png]]
	- So basically, we can do all the calculations on a computer and just transmit the real part of the analysis $$s(t) = \Re\{m(t)\}$$
	- So m(t) in the photo is the complex passband signal and s(t) is the real passband signal. So now, for all analysis we see it's complex. 
- Now, is the demodulation easy to do? We need to get m1 and m2 separately
	- We do something similar to the SSB demodulation by multiplying by a cos and passing through a LPF.![[Pasted image 20261020222128.png]]
	- multiply the qam signal by 2cos(wt) and 2sin(wt) and pass through LPF to get m1 and m2 respectively
# Applications of Amplitude Modulation
- AM Broadcast
	- audio 10kHz
	- bandwidth 20kHz
	- carrier freqs 540 kHz -> 1700 kHz
	- Different Tx power for day and night because of skywave propagation
- 