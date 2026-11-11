# Agenda
1.  Properties of Angle Modulation
2. Modulation (FM)
3. Demodulation (FM)
# Properties of Angle Modulation
- Effect of nonlinearity
	- $y(t) = as(t) + bs^3(t)$
		- We saw this isn't linear and adds distortion at the baseband
	- Now let's look at it for FM (that's $\psi$), it's basically the same analysis but Ac instead of m(t)![[Pasted image 20261109163315.png]]The bandwidth stays the same, and we can filter out the high frequency component, giving us just a change in amplitude for our original message
- Bandwidth
	- FM: $S_{FM}(t) = A_c\cos(\omega_ct + \int_0^t k_fm(t)dt)$
	- $\hat{S_{FM}}(t) = A_c \exp\{j[m(t)+k_fa(t)]\}$ where $a(t) = \int_0^tm(t)dt$
	- So, $S_{FM}(t) = \Re\{\hat{S_{FM}}(t)\}$ 
	- Taylor expansion of e^x = $\sum_{n=0}^{\infty}\frac{x^n}{n!} = 1 + \frac{x}{1} + \frac{x^2}{2!} + \dots$
	- So, let's evaluate this to get the bandwidth of our signal ![[Pasted image 20261109164805.png]]![[Pasted image 20261109164916.png]]
	- So... the bandwidth of our FM signal is technically infinity :shock:, so this is bad, right?
	- So the main problem is the a^n(t) terms, but what if kf was very small, if we raise it to n powers it'll tend closer to 0 for higher end powers. So, if k_f is reasonably small, we'll be fine, else we'll need other analysis techniques to see. 
# Modulation (FM)
- Do what we wrote, pass our signal into an integrator and then put it within a cos (oscillator) and change at w_c amplified by A_c. But this is complex, is there a way we can use AM approach to FM?
- So we talked about if k_f is very small, we can ignore the higher order terms, so we can just send the first two terms and we basically have DSB-SC modulation with a sin and a(t). So this is bandwidth efficient, and we call this Narrowband FM
- Narrowband FM (NOAA Weather)
	- If $|k_fa(t)| \lt\lt 1$ then $S_{FM} \approx A_c[\cos(\omega_ct) - k_fa(t)\sin(\omega_ct)]$ aka a DSB-SC with 50% phase shift, but only when the condition is MUCH smaller than 1
	- BW : 2B, same as DSB-SC/AM 
	- Modulator: ![[Pasted image 20261109165827.png]]
		- So, this is easy, and not that different to DSB-SC, but normally this isn't exactly what we want. We want a modulator for General FM Signals (wideband fm)
	-  General FM Signals (wideband fm)
		- BW of m(t): B
		- When we do the analysis, the problem is the integral, so what if we tried to make it a constant? Then, we'd know what the signal would look like in the frequency domain. So, we're going to decompose m(t) in parts with the rect function, aka sampling it, in order to analyze it. 
		- ![[Pasted image 20261109172446.png]]
		- So the window is 1/2B as it satisfies nyquist (just trust me bro) and we do this so we can have the integral evaluated and analyze the signal