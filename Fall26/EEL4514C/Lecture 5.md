# Agenda
- Complex Numbers
- Inner Products and Norms
- Signals and Time Averages
- Special Functions (complex exponential, Dirac delta, unit step)
- 
- Fourier Series
	- We need Fourier series and transform and LaPlace transform and all these transforms like dtft or z because periodic signals are easier to analyze and in order to decompose them, we do the Fourier series. now say we want to do the same analysis on a signal that is not periodic, so now we introduce the Fourier transform. then for signals that don't have a Fourier transform, we introduce the LaPlace transform that has a region of convergence that dictates if a signal can be converted or not.
# Complex Numbers
- A complex number is a pair of real numbers: $$z =(x,y) = x + jy = \Re\{z\} + j\Im\{z\} \qquad  j= \sqrt{-1} \qquad \text{ the imaginary unit}$$
- Cartesian form: $z = x + jy$
	- x = Re(z), the real part of z
	- y = Im(z), the imaginary part of z
	- x and y are also often called the *in-phase* and *quadrature* components of z
- Polar form: $z = re^{j\phi}$
	- r = |z| is the modulus or magnitude of z
	- $\phi$ is the angle or phase of z
	- ![[Pasted image 20260917234443.png]]
- Conversions:
	- ![[Pasted image 20260917234650.png]]
	- Addition: ![[Pasted image 20260917234718.png]]
	- Multiplication: ![[Pasted image 20260917234728.png]]
	- Conjugation: ![[Pasted image 20260917234735.png]]
	- (i don't feel like writing the latex for these, i'll write them for them for the new stuff)
- Euler's Formula
	- ![[Pasted image 20260917235351.png]]
	- Properties: ![[Pasted image 20260917235401.png]]
# Inner Products and Norms
- For complex signals f(t) and g(t), the inner product of f(t) and g(t) is$$\langle f, g \rangle = \int_{-\infty}^{\infty}f(t)g^*(t)dt$$
- For complex vectors **f** and **g**, the inner product of **f** and **g** is $$\langle \textbf{f, g} \rangle = \sum_i f_ig^*_i$$
- The norm-squared of a signal or vector $f$ is the inner product of $f$ with itself $$||f||^2 = \langle f, f \rangle$$
- For signals, this simplifies to $$\langle f, f \rangle = \int_{-\infty}^{\infty}f(t)f^*(t)dt = \int_{-\infty}^{\infty} |f(t)|^2 dt$$
- For vectors, this simplifies to $$\langle f, f \rangle = \sum_if_if^*_i = \sum_i|f_i|^2$$
- Functions or vectors $f$ and $f$ are orthogonal iff $$ \langle f, g \rangle = 0$$
# Signals and Time Averages
- The energy in a (possibly complex) signal $g(t)$ is $$||g(t)||^2 = E_g = \int_{-\infty}^{\infty}{|g(t)|^2 dt}$$
- For a real signal, this simplifies to $$||g(t)||^2 = E_g = \int_{-\infty}^{\infty}{g^2(t) dt}$$
- If $E_g < \infty$, then $g(t)$ is an energy signal
- All signals with finite amplitude *and* finite duration, then the signal is an energy signal
	- ![[Pasted image 20260918000743.png]](don't feel like writing \begin{cases})
	- square wave has finite duration and amplitude, so energy signal
- Note that an energy signal does not have to have finite duration
	- ![[Pasted image 20260918000817.png]]
	- exponential decay signal having energy since less than infinity
- Nor does it have to have finite amplitude:
	- ![[Pasted image 20260918003149.png]]
- In communications, we often use signals that have $E = \infty$
	- Example:![[Pasted image 20260918003345.png]]
- So how should we handle such signals? Consider the time-average of energy:
	- The *power* in a signal $g(t) is $$P_g = \lim\limits_{T \to \infty} \frac1T \int_{-T/2}^{T/2}g^2(t)dt$$
- Whenever you see a limit, you have to be concerned about when the limit exists:
	- Periodic signals: A signal is periodic if there exists T such that $g(t) = g(t + T)$ for all $t \in \mathbb{R}$. The smallest such T is called the period of g(t)
	  If g(t) is a periodic signal with period T, then $$P_g = \frac1T \int_{-T/2+a}^{T/2+a}g^2(t)dt, \qquad \forall a \in \mathbb{R}$$
- Certain random signals that possess *statistical regularity*
- g(g) is a power signal if $0 \lt P_g \lt \infty$ (both nonzero and finite)
- **Note:** All real-world signals are energy signals because power signals must be nonzero over infinite time
- However, power signals provide useful models and analysis tools of dealing with real signals.
- **If a signal has energy, the power would be 0, which marks the difference between energy and power**
- Another common time average for power signals is:
	- The **DC Value** of g(t) is:$$g_{DC} = \lim\limits_{T \to \infty} \frac1T \int_{-T/2}^{T/2}g(t) dt$$
	  (Generally do not want $g_{DC} \ne 0$ for communication systems. Results in transfer of power that does not convey any info.)
# Special Functions
- Complex Exponentials
	- $x(t) = e^{j \omega t} = \cos(\omega t) + j\sin(\omega t)$
	- Magnitude = r = |x(t)| = 1
	- Phase: $\phi = \omega t + 2k\pi, t \in \left (\frac{-\pi - 2k\pi}{\omega}, \frac{\pi - 2k\pi}{\omega} \right ), k \in \mathbb{Z}$
	- Complex exponentials are periodic signals with fundamental period of $\frac{2\pi}{\omega}$
	- Complex exponentials are power, non-causal signals 
	- ![[Pasted image 20260918012957.png]]
- Unit impulse/ Dirac delta
  Formally, continuous-time unit impulse signal (dirac delta function) $\delta(t)$ is defined by the property that $$\delta(t) = \begin{cases} \infty, \qquad t = 0 \\ 0, \qquad t \ne 0 \end{cases}$$and $$\int_{-\infty}^{\infty}\delta(t)dt=1$$
	- ![[Pasted image 20260918013415.png]]
- Dirac delta function is an idealization of a signal that 
	- is very large near t=0
	- is very small away from t=0
	- has integral 1
	- e.g. where $\epsilon$ is small![[Pasted image 20260918013503.png]]
- Plot $\delta(t)$ as![[Pasted image 20260918013517.png]]
	- Formally, discrete-time unit impulse signal (kronecker delta function) $\delta[n]$ is defined by the property that $$\delta[n] = \begin{cases} 1, \qquad n = 0 \\ 0, \qquad n \ne 0\end{cases}$$
- Properties
	- If we have a constant times the delta function![[Pasted image 20260918013815.png]]
	  Note that c is not the value of $c\delta(0)$. That is undefined (we may imagine it to be $\infty$). instead, we say that c is the **weight** of $c\delta(t)$
	- $g(t)\delta(t) = g(0)\delta(t)$. This easily extends to
	- $g(t)\delta(t-T) = g(T)\delta(t-T)$ i.e. since $\delta(t-T)$ is only nonzero at T, that is the only value of g(t) that is preserved after multiplication by the $\delta$
	- 
