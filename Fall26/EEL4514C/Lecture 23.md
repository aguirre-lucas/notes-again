# Agenda
1. PLL in Presence of Modulation
2. Superheterodyne Receiver
3. Applications of FM
# PLL in Presence of Modulation
- So here, we assume the input signal is our FM signal. The one reason we can't do the incoherent detection in AM modulation is because at the receiver we can't get the exact phase. Now, it turns out that we can use PLL to generate a local carrier with the correct phase from the received signal. 
-  So, the goal here is to generate the local carrier with the right phase in order to do the coherent demodulation. So if we transmit AM signal, DSB-SC signal with carrier, if we do that in addition to the envelope detector, we can create a narrow filter in order to filter the carrier out and use that to do the coherent detection. But normally, the quality of the carrier might be bad because of that narrow filter and potentially filter out the message information. 
- DSB-SC: $m(t)\cos(\omega_ct + \phi)$
- Good: $A\cos(\omega_c t + \phi)$
	- the $\phi$ want to be as close as possible, but if there's a small difference it's ok
	- now we have m(t) instead of a constant A like in our previous analysis, so we don't want m(t) when generating the local carrier
	- Two approaches to doing this
		1. Squaring Loop
		2. Costas Loop
- Squaring Loop
	- ![[Pasted image 20261110153110.png]]This allows for incoherent demodulation by creating the carrier!
- Costas Loop (simpler)
	- It's similar to QAM having I/QSo now our $v_3(t)$ will look like![[Pasted image 20261110160104.png]]
		- So now we've regenerated the carrier![[Pasted image 20261110160049.png]]
# Superheterodyne Receiver
- Problem: Need bandpass filters around desired signal, but it is hard to build tunable filters that are very selective (high Q)
	- So, moving a filter in a frequency band is pretty simple, using a complex exponential. So, building a high Q filter on a certain frequency band and move it in the frequency domain to have a high quality filter across all the frequency bands we want. 
- Superheterodyne uses a fixed, high Q bandpass filter at an intermediate frequency (IF). 