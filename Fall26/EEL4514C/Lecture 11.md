# Agenda
- Power/Energy in the Frequency Domain
- Energy Spectral Density (ESD)
# Power/Energy in the Frequency Domain
- Signal space representation are inner-product and norm preserving (fourier transforms/series)
- If s(t) can be represented as $$ s(t) = \sum_{n}S_nf_n(t) $$ where $f_n(t)$ is finite-energy orthonormal basis $$E_s = \int_{-\infty}^{\infty}|s(t)|^2dt = \sum_n|S_n|^2 = ||S_n||^2$$
- Complex-exponential F.S. 
	- $s(t)$ in time that's periodic $$ \begin{align} P_s &= \frac{1}{T_0} \int_{a}^{a+T_0}|s(t)|^2dt \\ &= \frac{1}{T_0} \int_{a}^{a+T_0}\sum_n|S_ne^{j2\pi n f_0 t}|^2 dt \\ &= \frac{1}{T_0} \int_{a}^{a+T_0}\sum_n|S_n|^2 \cdot |e^{j2\pi n f_0 t}|^2 dt \qquad \text{complex sinusoid resolves to 1 for magnitude} \\ &= \frac{1}{T_0} \int_{a}^{a+T_0}\sum_n|S_n|^2 dt \\ &= \frac{1}{T_0} \sum_n|S_n|^2 \int_{a}^{a+T_0}1dt \\ &= \frac{1}{T_0} \sum_n|S_n|^2 T_0 \\ &= \sum_n|S_n|^2\end{align}$$So the power of the signal is equal to the norm of the fourier series coefficients, which is normal in the frequency domain. 
	- So if I wanted to compute the power of the signal $$P_s = \frac{1}{T_0} \int_{a}^{a+T_0}|s(t)|^2dt = \sum_n|S_n|^2$$ Where the integral is the time domain and the sum is the frequency domain. The sum is basically the definition of energy, so the time domain power is the energy in the frequency domain. **This is called Parseval's Theorem!***
	- e.g.
		- ![[Pasted image 20261016180747.png]]
		- It's easier to calculate energy in freq than power in time
	- This showed it was norm-preserving, let's prove it's inner-product preserving
	- Fourier Transform is inner-product preserving (not writing this one out)![[Pasted image 20261016181111.png]]
	- Inner product in the time domain and inner product in the frequency domain
	- Corollary $$E_g = ||g(t)||^2 = ||G(f)||^2$$Which is equal to $$<g(t), g(t)> \space = \space <G(f), G(f)>$$
- 