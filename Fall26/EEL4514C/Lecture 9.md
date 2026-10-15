# Agenda
1. Fourier Transform of Periodic Signals
- With the introduction of the Dirac delta function, we do *not* need the Fourier series, so we can compute the Fourier transform of a periodic signal.  
2. Bandwidth
3. More abt FT Properties (that are important for this class)
# Fourier Transform of Periodic Signals
- ![[Pasted image 20260921013256.png]]
	- Basic review, not much to say. It's nice cuz the exponential is a function of T and because of delta we can do a Fourier transform of *at least* that periodic function. 
- Fourier transform of a general periodic function
	- s(t) denotes a periodic signal w/ fundamental period $T_0 = 1/f_0$ 
	- when we compute the fourier transform of a fourier series, we normally write down the fourier series first
	- $$s(t) = \sum_{n=-\infty}^{\infty} S_ne^{j2\pi n f_0 t}$$This is the fourier series of s(t). We write this because if we do a fourier transform of it, we know the FT is a linear operator, so we can move it inside the sum so $$= \sum_{n=-\infty}^{\infty}S_n\mathcal{F}\{e^{j2\pi n f_0 t}\}$$ Therefore $$S(f) = \sum_{n=-\infty}^{\infty}S_n \delta(f-nf_0)$$So basically this is the Fourier transform of a periodic signal. It's very similar to the FS, just replace the complex exponential to the delta function. **Be aware that the first equation is the time-domain and the final one is the frequency domain.**
- e.g. For rectangular pulse waveform (below is S(f))![[Pasted image 20260921014343.png]]
	- $S_n = \frac{1}{T_0}G(\frac{n}{T_0}) = f_0G(nf_0)$
	- $= \frac{\tau}{T_0} \sinc \left ( \frac{\pi \tau n}{T_0}\right )$
	- The red part is basically the fourier transform S(f) 
- Special example $\tau = T_0$![[Pasted image 20260921014449.png]]This is our s(t), or g(t) whatever you want to call it![[Pasted image 20260921014539.png]]
	- so basically all the zero points are sampled every sample. so in the frequency domain, it simplifies to just a delta signal![[Pasted image 20260921014734.png]]
	- So basically, if $\tau$ and $T_0$ are the same, what we have in the time domain is![[Pasted image 20260921014816.png]]Which becomes a constant in the time domain, which means in the frequency domain becomes a delta signal. This is a special case!
- Another special case of periodic signals (special meaning EXTREME)
	- This example will violate Dirilecht conditions![[Pasted image 20260921015248.png]]
	- $$s(t) = \sum_{k=-\infty}^{\infty}\delta(t-kT_0)$$Can we perform the Fourier transform of this? Yes! We can turn this into a Fourier series, then use linearity to do the Fourier transform
	- $$S(f) = \sum_{k=-\infty}^{\infty}S_ne^{j2\pi n f_0t}$$Where this is our Fourier series
	- $$S_n = \frac{1}{T_0}\int_{-T_0/2}^{T_0/2} \delta(t)e^{j2\pi n f_0 t}df$$As we integrate for one period, we see that in the graph only one delta function exists per period. So this entire equation simplifies to 1! $$S_n = \frac1{T_0}$$Therefore the Fourier series coefficient is this. Since it's not a function of n, it's a constant for all complex exponentials
	- $$\mathcal{F}\left \{\frac{1}{T_0}\sum_{n=-\infty}^{\infty}e^{j2\pi n t/T_0} \right \}$$Which we move it inside around the complex exponential which simplifies to $$\frac1{T_0}\sum_{n=-\infty}^{\infty}\delta(f-\frac n{T_0}) = f_0\sum_{n=-\infty}^{\infty}\delta(f-nf_0)$$ So this is the Fourier transform of the extreme case. What we have is something weird, so if we do a time domain signal that is an "impulse train", the in the frequency domain, it is also an "impulse train". ![[Pasted image 20260921020046.png]]
	- This formula is extremely important when we do digital communication. When we do digital communications, we convert an analog signal, or continuous time signal, to discrete time. In order to do that is we multiply the continuous time signal by an impulse train, aka the function we just worked on. So in the time domain we have a multiplication, but in the frequency domain we have a convolution. We'll talk abt this in the second half of the class
	- ![[Pasted image 20260921021148.png]]
		- If we do a summation of a bunch of delta signals, it's equal to a summation of a bunch of complex equations. If we do a Fourier transform of a bunch of delta signals, it's another summation of complex exponentials 
# Bandwidth
- So there's no unique way to define bandwidth. But the basic idea of bandwidth is that we want to keep most of the signal within a certain frequency band. How much we're gonna keep it depends, sometimes we only need half of the energy portion, but sometimes we want more. 
- E.g., Baseband square pulse
	- $g(t) = \sqcap(t/\tau)$
		- $\sqcap (t) = \begin{cases} 1, |t| \le 1/2 \\ 0, \text{else}\end{cases}$ ![[Pasted image 20260921022343.png]]
		- $G(t) = \tau\sinc(\pi f \tau)$ ![[Pasted image 20260921022448.png]]
			- So here, we denote B as our bandwidth. 
		- B = the first zero bandwidth = 1/$\tau$
- E.g. Modulated square pulse
	- $g_2(t) = g(t)\cos(2\pi f_c t)$ where $f_c$ is the carrier frequency
	- ![[Pasted image 20260921022902.png]]
		- Tied back to the thing 2 lectures ago about frequency shifting!! Results in![[Pasted image 20260921022959.png]]
		- So now, the bandwidth of this signal can be defined as the two zeros next to $\pm f_c$ ![[Pasted image 20260921023058.png]]
		- So this is the null-to-null B = 2/$\tau$
	- So the first bandwidth we saw we called the base bandwidth as it's around 0, but as we do the modulation and shift it in the frequency domain, we can see the negative frequencies before become positive, so we've doubled the bandwidth, which is 2 times the base bandwidth of the signal. Meanwhile, this negative component does not carry any new information. 
# More Properties of the Fourier Transform
1. Time Scaling
	- ![[Pasted image 20260921023903.png]]
- 