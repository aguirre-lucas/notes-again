# Agenda
1. Modulation (FM) for Wideband FM
2. Demodulation (FM)
# Modulation (FM) for Wideband FM
- $S_{FM}(t) = A_c\cos(\omega_ct + k_f\int m(\alpha)d\alpha)$ 
- ![[Pasted image 20261109173828.png]]
	- So a single chunk is a square wave, which is a sinc() function in frequency. So, in the frequency domain, we just need to add all of these, which are shifted square waves aka sinc() functions in frequency. We know a time shift is just a phase shift in frequency, so it won't impact the bandwidth of the signal.
- $\hat{s}(t) = \cos(\omega_ct + k_fm(t_k)t)\sqcap(\frac{t}{\tau})$ (the equation is meant to say sinc not sin)![[Pasted image 20261109175104.png]]So this is just one chunk of that entire sampling problem we need to solve. So, for the entire signal, we can measure the bandwidth 
- ![[Pasted image 20261109175350.png]]
	- So here BW: $2[k_fm_p+1/T]$ where T=1/2B, so $2[k_fm_p+2B]$
	- $\Delta f = k_f \frac{m_{max} - m_{min}}{4\pi}$, which we call the peak frequency deviation, or what is the max deviation of the frequency.
	- BW: $2[\Delta f+2B]$
		- ^^ This is too conservative tho
	- A better estimation, **Carson's Rule**: $BW_{FM} \approx 2[\Delta f + B]$ 
- Frequency Modulation Index, $\beta$  , is the peak frequency deviation, $\Delta f$ , expressed a a proportion of message bandwidth. 
	- $\beta = \frac{\Delta f}{B}$
	- $B_{FM} \approx 2B ( \frac{\Delta f}{B} + 1)$
	- $\approx 2B(\beta + 1)$
	- This is now in the form of the DSB-SC/AM bandwidth! So we say FM requires 1+ $\beta$ bandwidth than DSB-SC
	- For narrowband FM signal, the bandwidth is approximately
		- $k_f$ is small --> $\Delta f$ is small --> $\beta$ << 1 , so $B_{NFM} \approx 2B(0+1) \approx 2B$ 
# Demodulation (FM)
1. Differentiation + Envelope Detector
	- Low complexity
	- $$\begin{align} S_{FM}(t) &= A_c\cos \left (\omega_ct + k_f\int m(\alpha)d\alpha \right ) \\ \frac{dS_{FM}(t)}{dt} &= A_c \left [\omega_c t + k_fm(t) \right ] \cdot \sin \left(\omega_ct + k_f\int m(\alpha)d\alpha \right )\end{align}$$Where we get a constant A_c\*w_c and A_c\*k_f\*m(t). So the message is in the amplitude, like AM, so we can use an envelope detector to demodulate the message.
	- Envelope detector gives: $A_c(\omega_c + k_fm(t)$, but A_c isn't really constant in practice, it varies with with time t and there will be some distortion. in order to remove this, we use a Bandpass Limiter
	- ![[Pasted image 20261110131434.png]]Some problems with this are that implementing an ideal differentiator may be hard and we cannot convert FM signal to AM signal, so the disadvantages of AM also apply here. Therefore, we may be able to improve on this design. 