# Agenda
1. Energy Spectral Density
2. Power Spectral Density
# Energy Spectral Density
- Autocorrelation function $$\begin{align} \psi_g(\tau) &= <g(t), g(t-\tau)> \\ &= \int_{-\infty}^{\infty}g(t)g^*(t-\tau)dt \\ &= \int_{-\infty}^{\infty}g(u+\tau)g^*(u)du \\ \text{assume g(t) is real, conjugate's the same} &= \int_{-\infty}^{\infty}g(u-(-\tau))g(u)du \\ \psi_g( -\tau) &= \int_{-\infty}^{\infty}g(u-(-\tau))g(u)du \end{align}$$
	- $\psi_g(\tau) = \psi_g(-\tau)$ when $g(t)$ is real, making it an even function
	- It also looks very similar to convolution, just t-tau not tau-t
	- Let $\tilde{g}(\tau)$ be g(-t) $$\begin{align} \psi_g(\tau) &= \int_{-\infty}^{\infty}g(t)\tilde{g}(\tau-t)dt \\ &= g(\tau) * \tilde{g}(\tau) \end{align}$$So we can write the autocorrelation function as a convolution of g(tau) with ~g(tau). let's do the Fourier transform $$\begin{align} \mathcal{F}\{ \psi_g(\tau) \} &= \mathcal{F}\{g(\tau) * \tilde{g}(\tau)\} \\ &= \mathcal{F}\{g(\tau)\}\mathcal{F}\{\tilde{g}(\tau)\} \\ &= G(f)G^*(f) \\ &= |G(f)|^2 \rightarrow \text{ESD} \\ \rightarrow \mathcal{F}\{ \psi_g(\tau)\} &= |G(f)|^2\end{align}$$ The fourier transform of the autocorrelation function is the energy spectral density, so basically if we do an IFT of the ESD we get the autocorrelation function, so if ESD is hard to compute, we can do it in the time domain and do the fourier transform to get the ESD.  Autocorrelation = Time-Domain ESD
# Power Spectral Density (PSD)
- Consider g(t) is a power signal with power $P_g = \lim\limits_{T \rightarrow \infty} \frac1T \int_{-T/2}^{T/2}|g(t)|^2dt$
	- that 1/T coefficient is annoying, so let's move T inside the g(t)
	- Let $g_T(t) = \begin{cases} g(t), \qquad t \le \frac{|T|}{2} \\ 0, \qquad \text{else} \end{cases}$
		- This makes it an energy signal
	- So $P_g = \lim\limits_{T \rightarrow \infty}\frac1T \int_{-\infty}^{\infty}|g_T(t)|^2dt$
		- Now we can replace this with the ESD
	- $P_g = \lim\limits_{T \rightarrow \infty}\frac1T \int_{-\infty}^{\infty} |G_T(f)|^2df$ 
		- Now let's exchange the order of the limit and integral
	- $P_g = \int_{-\infty}^{\infty} \lim\limits_{T \rightarrow \infty}\frac1T |G_T(f)|^2df$
		- So this similar to something we've seen before with energy, and now the power is the integral over the entire frequency with something inside. So, we can define this as a density, specifically as the PSD
- Definition (PSD):
	- For a power signal g(t) the Power Spectral Density (PSD) is $$S_g(f) = \lim\limits_{T \rightarrow \infty} \frac{|G_T(f)|^2}{T}$$One problem, this is hard to compute since we need to truncate the power signal and then compute the FT and then set to infinity and it's not an easy process
	- Some remarks, if g(t) is real it's symmetric in the freq domain so $P_g = 2 \int_0^{\infty}S_g(f)df$ the power in band (f_1, f_2) $P_g = 2\int_{f_1}^{f_2}S_g(f)df$
- So how can we compute the PSD?
	- Definition (autocorrelation function for power signals)
	- Let g(t) be a power signal, then the autocorrelation function is $$R_g(\tau) = \lim\limits_{T \rightarrow \infty} \frac1T \int_{-T/2}^{T/2}g(t)g^*(t-\tau)dt$$ Then $$S_g(f) = \mathcal{F}\{R_g(\tau)\}$$This is called **Wiener-Khinchin Theorem**, and it's normally used when g(t) is a random process or signal, but here g(t) is deterministic. 
	- So we're gonna compute the time domain signal of the autocorrelation function first and then do the FT
	- Special Case! Periodic Signals
		- g(t) is a real periodic signal with period $T_0$
		- $$\begin{align} R_g(\tau) &= \lim\limits_{T \rightarrow \infty} \frac1T \int_{-T/2}^{T/2}g(t)g^*(t-\tau)dt \\ &= \lim\limits_{n \rightarrow \infty} \frac1{2nT_0} \sum_{k=-n}^{n-1}\int_{kT_0}^{(k+1)T_0}g(t) \times \left [ \sum_{m=-\infty}^{\infty}S_me^{j2\pi mf_0 (t-\tau)} \right ]^*dt \\ &=  \lim\limits_{n \rightarrow \infty} \frac1{2n} \sum_{m=-\infty}^{\infty}S_m^*e^{j2\pi mf_0 \tau}\sum_{k=-n}^{n-1}\frac{1}{T_0}\int_{kT_0}^{(k+1)T_0}g(t)e^{-j2\pi mf_0 t} dt \\ &= \lim\limits_{n \rightarrow \infty} \frac1{2n} \sum_{m=-\infty}^{\infty}S_m^*S_me^{j2\pi mf_0 \tau}2n \\ &= \lim\limits_{n \rightarrow \infty} \sum_{m=-\infty}^{\infty}S_m^*S_me^{j2\pi mf_0 \tau} \\ &= \sum_{m=-\infty}^{\infty}S_m^*S_me^{j2\pi mf_0 \tau} \\R_g(\tau) &= \sum_{m=-\infty}^{\infty}|S_m|^2e^{j2\pi mf_0 \tau} \end{align}$$Looks a lot better to compute, and it's basically the same as the Fourier series of g(t). So if we write the autocorrelation as the fourier series, the fourier transform is ![[Pasted image 20261018181804.png]]
	- e.g. Power in unipolar square wave![[Pasted image 20261018181947.png]]
	- First, compute G(f) = $\tau sinc(\pi f \tau)$ where $\tau = \frac{T_b}{2}$
	- Second, compute the fourier series coefficient $S_n = \frac1{T_b} \cdot G(\frac{n}{T_b})$
		- $= \frac1{T_b} \cdot \frac{T_b}{2}sinc(\pi \frac{n}{T_b} \frac{T_b}{2})$
		- $= \frac12 sinc(\frac{\pi n}{2})$
		- then, square the coefficient
		- $|S_n|^2 = \frac{1}{4}sinc^2(\frac{\pi n}{2}) = \begin{cases} \frac14, \qquad n = 0 \\ \frac1{n^2\pi^2}, \qquad n \text{ is odd} \\ 0, \qquad \text{otherwise} \end{cases}$
		- ![[Pasted image 20261018183058.png]]
	- Lastly, write the PSD
		- $S_g(f) = \sum_n|S_n|^2 \delta(f-\frac{n}{T_b})$
