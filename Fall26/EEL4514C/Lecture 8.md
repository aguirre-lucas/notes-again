# Agenda
1. Connecting FT & FS
2. FT of a periodic signal (didn't get to it)
# Connecting FT and FS
- Use FT to calculate FS
	- Let s(t) be a periodic signal $$s(t) = \sum_{n = -\infty}^{\infty} g(t-nt_0)$$![[Pasted image 20260920172336.png]]
	- $$\begin{align} S_n &= \frac1{T_0}\int_{-T_0/2}^{T_0/2}s(t)e^{-\frac{jn2\pi t}{T_0}}dt \\ &= \frac1{T_0}\int_{-T_0/2}^{T_0/2}g(t)e^{-\frac{jn2\pi t}{T_0}}dt \\ G(t) &= \int_{-\infty}^{\infty}g(t)e^{-j2\pi ft} dt \\ \implies S_n &= \frac1{T_0}G\left ( \frac n{T_0} \right ) &= f_0G(nf_0)\end{align}$$So basically if we know the Fourier transform of G() then we can compute the Fourier series using this formula. 
- E.g. Rectangular Pulse Waveform
	- ![[Pasted image 20260920173256.png]]
	- Let $\sqcap (t) = \begin{cases} 1, \qquad |t| \le 1/2 \\ 0, \qquad \text{else} \end{cases}$ ![[Pasted image 20260920173857.png]]
	- g(t) = $\sqcap \left ( \frac{t}{\tau}\right )$ 
	- G(t) = $\mathcal{F} \{\sqcap(\tau/2)\}$ = (below) = $\frac{\sin(\pi f \tau)}{\pi f} = \tau sinc(\pi f \tau)$  ![[Pasted image 20260920175612.png]]Which looks like![[Pasted image 20260920175827.png]]where x=0 is the peak and all the crossing points are the inverse of $\tau$
	- FS Coefficients $$S_n = \frac{1}{T_0}G(n/T_0) = \frac{\tau}{T_0}sinc \left (\frac{\pi n \tau}{T_0} \right ) \qquad T_0 \ge \tau$$So if we draw the frequency response of this periodic signal here G(), all these components are the samples of the inverse(tau) points. The dotted line is the envelope and for all S_n we take all points of the envelope to reconstruct the square wave. ![[Pasted image 20260920180256.png]]The sampling points are 1/T_0 but the multiples of tau are 0, so $S_n = 0 \qquad \text{if} \qquad k/\tau = m/T_0 \qquad k,m \in \mathbb{Z}$ . A special case of $T_0 = 2\tau \implies 1/T_0 = 1/2\tau$ so now for every envelope we only have 1 point and sample all the zero points![[Pasted image 20260920180651.png]]
- Now, if we shift it by like a factor of 1/$\tau$ to the right or left, if we take the point at any point of n/$\tau$, there's no interference from any other waveform, meaning we get a clean signal. so then, if we shift it in the frequency domain 1/$\tau$, we multiply by a complex exponential in the time domain of $f_0 = 1/\tau$ . 
  ![[Pasted image 20260920183025.png]]
	- Ji's words: "If we just a square wave for example \[points to one given in example shown earlier\] as the plus 1 and flipped as minus 1 and then we multiply each guy by a different complex exponential corresponding to this frequency shift \[$1/\tau$\] and add all of them together. and then what we do is this frequency sampling, and then we do not have any interference. it's a very easy way to do it. the beauty of that is  that basically you can see the shifting is 1 over tau, so if we make tau small, we can pack lots of this type of signal into a certain bandwidth and this is called OFDM, and this is now what we use for 4G 5G 6G WiFi" 
		- "This is a very simple concept" - Ji :sob:. Hopefully it becomes simple soon
	- My reasoning:
this is what my understanding is at rn but i'll understand it better soon hopefully:

basically by making the pulse periodic by repeating g(t) every T_0, the fourier series coefficients of the waveform are samples of the single-pulse fourier transform 1/T_0 * g(n/T_0) connecting the fourier transform to the fourier series. the sinc curve G(f) is the "envelope" (ji said that), the fourier seires takes it's discrete values of it at f_n = n/T_0 and divides it by T_0 to reconstruct the periodic rectangular waveform. now, we know that the max value of a sinc() wave is at 0, with a value of tau. if we set T_0 to equal 2 * tau, the fourier series sample spacing is 1/2 * tau. and since the sinc() zeros occur at every 1/tau, that means it'll be zero at every k/tau, or 2k/T_0, meaning every even-indexed fourier coefficient except DC is 0. 

NOW with that basis, we can shift in the frequency domain by 1/tau (multiply by a complex exponential in the time domain). in OFDM, subcarriers are a narrowband sinusoid that holds *one* complex data point during one OFDM symbol time. now, with the frequency shifts, we can sample at the same time for all points without the signals interfering at different frequencies, allowing for orthogonality* in data flow and faster data transfer, making up the backbone of like 5G and 6G and wifi and stuff.