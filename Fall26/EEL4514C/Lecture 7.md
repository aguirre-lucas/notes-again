- Last lecture, we covered the periodic case for representing a signal as a sum of signals at individual frequencies, now we'll cover the second case of aperiodic signals
-  This is insanely good![[Pasted image 20260919200836.png]]If we consider our input like this, and know s_n, we can get H() easily, as for each frequency component we get our output and divide it by our input and get our "channel", aka the system. so this makes the channel estimation for wireless communication extremely easy. this is the backbone for 5G 6G and OFDM (orthogonal frequency division multiplexing). but, there's a problem that we'll cover later in this class where in practice we don't have a real periodic signal because of truncation
# Agenda
- Frequency Response of Non-Periodic Signal in LTI System
- Energy Signals in LTI Systems
- Negative Frequency
- Common Fourier Transforms
- Frequency Shifting Properties
- Connecting Fourier Transform and Fourier Series
# Frequency Response of Non-Periodic Signal in LTI System
- Case 2: Non-periodic signals
	- The impulse response above is an example of a non-periodic energy signal
	- To represent such signals in the frequency domain we need an even bigger basis: $$\{e^{j2\pi ft}, f \in \mathbb{R} \leftarrow \text{ the whole real line}.\}$$
	- Then the representation of h(t) in the frequency domain has values at every $f \in \mathbb{R}$, and we write it as H(f)
		- before we only had points in the frequency domain as points of n times the fundamental frequency, aka discrete. now, we make it continuous to capture all of the frequency as the fundamental period is $\infty$ due to not having a period
	- As usual, we can get the value H(f) by projecting h(t) onto the basis function at frequency f![[Pasted image 20260920152813.png]]
	- H(f) is the Fourier Transform of h(t)
	- We can reconstruct h(t) in terms of these frequency components as![[Pasted image 20260920153129.png]]
	- h(t) is the Inverse Fourier Transform of H(f):
		- Inverse Fourier Transform: For a frequency-domain signal H(f) or H($\omega$), the *Inverse Fourier Transform*, denoted $h(t) = \mathcal{F}^{-1}\{H(f)\}$ or $h(t) = \mathcal{F}^{-1}\{H(\omega)\}$ is given by![[Pasted image 20260920153245.png]]
	- We will generally use Hertz (f) because most communication frequencies are specified in Hz and because we don't have to worry about the extra factor of 1/(2$\pi$)
	- Each h(t) corresponds to a unique H(f) and vice versa, so we call them a *Fourier pair* and write ![[Pasted image 20260920153400.png]]
	- In Hz, the Fourier Transform and Inverse Fourier Transform differ only by the sign of the exponential term, so they give almost identical results. 
		-  given h(t) and H(f) are similar. 
	- Note also that the Fourier Transform and IFT are linear operators![[Pasted image 20260920155449.png]]
# Energy  Signals in LTI Systems
- Let s(t) be an energy signal with Fourier Transform S(f)
- Let s(t) be input to an LTI system with impulse response h(t), which ash frequency response H(f)
	- note we use Fourier series for periodic signals, and Fourier transforms for aperiodic signals. assume s(t) is aperiodic. 
- Then the output is $r(t) = [s * h](t)$, and![[Pasted image 20260920155623.png]]
- Thus, R(f) = H(f)S(f), where R(f) is the FT of r(t)
	- So in the time domain it's a convolution, and in the frequency domain it's a multiplication
# Negative Frequency
- Read section 2.9.2 (after this lecture)
- For a real signal, consider how the frequency response at some frequency $-f_0 \lt 0$ relates to the frequency response at $f_0 \gt 0$![[Pasted image 20260920160057.png]]
	- we consider g(t) to be a real signal, so if we take the negative frequency, after some manipulation,, we see that G(-f0) becomes the conjugate $G^*(f_0)$  
	- on the high level, the negative frequency does not bring any new information, so by only knowing the positive part is (basically) enough. So, the negative and positive part are somehow identical (**for a real signal**)
	- later on, what he's saying is not accurate but for now just take it as face value
- If g(t) is real, $G(-f_0) = G^*(f_0)$ 
- If we consider the contributions to the Synthesis formula at both $f_0$ and $-f_0$, we get:![[Pasted image 20260920160717.png]]
  which is a real signal
- Thus, the positive and negative frequency terms in the exponential FS/FT combine to give real sinusoids
- When we measure bandwidth, we only care about real sinusoids, so we can just measure in the positive frequency domain
- ![[Pasted image 20260920162939.png]]
	- Here, we say the bandwidth is W, not 2W since the negative component is redundant
# Common Fourier Transforms
- Tables in FDSP so I won't get too into this
	- $\mathcal{F} \{\delta(t)\}= 1$ 
		- For a system with impulse response h(t) = $\delta(t)$, the frequency response is H(f) = 1. When you do the integral $\delta(t)$ only has a value at time 0, which simplifies the complex exponential to 1, thus giving one.
			- So if the input is s(t) and the output is r(t), then R(f) = 1\*s(f), so r(t) = s(t)
	- $\mathcal{F}^{-1} \{\delta(f-f_0)\}= e^{j2\pi f_0 t}$ 
		- Here, we use the IFT because we always shift in the frequency domain, not the time domain
	- Similarly, $\mathcal{F}^{-1} \{\delta(f+f_0)\}= e^{-j2\pi f_0 t}$
- From Euler's formula:![[Pasted image 20260920165122.png]]
	- There was something weird. The Fourier transform of a complex exponential is a delta function. BUT, the complex exponential is cos + j\*sin, so it's a periodic signal. so what we just did was compute the Fourier transform of a periodic signal, even though we said the Fourier transform was for aperiodic signals, aka we just contradicted ourselves. the reason for this is that for introducing the delta signal, a so called generalize function, we can do a Fourier transform of a periodic signal.  
	- therefore, we can actually do a Fourier transform on a periodic signal, like cos and sin. 
- Then![[Pasted image 20260920165524.png]]
	- Basically we can do a Fourier transform of a cos instead of a Fourier series. 
	- In practice, we don't use the FS that much, we use FT as it can cover periodic signals
- Then![[Pasted image 20260920165712.png]]
- Similarly
  ![[Pasted image 20260920165724.png]]
  and![[Pasted image 20260920165741.png]]
# Frequency Shifting Properties
- Please review all the properties in the book!!! Ji only covers the frequency shifting in class cuz that's used the most. 
- Let g(t) be an energy signal with g(t) <-> G(f)
- Consider![[Pasted image 20260920165842.png]]
	- So if we're in the time domain, if we multiple a signal by a complex exponential, in the frequency domain we do a **frequency shift**.
	- This is nice because in communications, the channel cannot allow all frequencies to pass. Considering a real channel, if a signal passes through an LTI signal, the system multiplies this guy and becomes the output. If we wanted to pass a signal like the s(t) with H(f), then we get 0 for all frequency components. BUT, if we shift s(t), we can get the signal to pass through H(f)!!![[Pasted image 20260920170111.png]]**This is called modulation in communications**
- It's kind of weird to multiply a signal by a complex exponential, but multiplying by a real signal like cos(), we get ![[Pasted image 20260920170155.png]]two components here that display what was shown above. So instead of multiply s(t) by a complex exponential, we multiply by like cos(omega\*t) with half the amplitude (1/2 in the front of the fourier transform result) but it now passes through the channel H(f). ![[Pasted image 20260920170254.png]]
