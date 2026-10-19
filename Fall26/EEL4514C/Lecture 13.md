# Agenda
1. Power Spectral Density
2. Modulation
# Power Spectral Density
- Special case: Random Power Signal
	- $$\begin{align} R(\tau) = \lim\limits_{T \rightarrow \infty}\frac1T\int_{-T/2}^{T/2}g(t)g^*(t-\tau)dt \end{align}$$Let's make some assumptions
		- If T->infinity and g(t) is ***ergodic*** which in simplest and incomplete terms means R(t) will be the expectation of $g(t)g^*(t-\tau)$ but you have to assume g(t) is an Ergodic Process
# Modulation
- General definition: **The mapping of information to a signal that is appropriate for transmission over channel**
- Carrier Modulation   
	- What is a carrier? In this class, it's a sinusoid signal (cos()) used to map the PSD of a message signal to a different frequency band
	- The frequency of a carrier is called the carrier frequency $f_c$ , sometimes $f_0$
	- The simplest form of the carrier modulation is $$s(t) = m(t)\cos(2\pi f_ct)$$Where s(t) is the modulated signal, m(t) is the message signal, and the cos() is the carrier. ![[Pasted image 20261018194744.png]]
	- ![[Pasted image 20261018194804.png]] A type of amplitude modulation
	- $$\begin{align} S(f) &= M(f) * \frac12(\delta(f-f_c) + \delta(f+f_c)) \\ &= \frac12M(f-f_c) + \frac12M(f+f_c)\end{align}$$![[Pasted image 20261018195000.png]]
- General Carrier Modulation $$s(t) = A(t) \cos \left ( 2\pi \left [ f_c + \Delta f(t) \right ] + \phi(t) \right ) $$
- Three types of analog modulation
	- Amplitude modulation (AM Radio)
		- A(t) varies linearly with m(t)
	- Frequency modulation (FM Radio)
		- $\Delta f(t)$ varies linearly with m(t)
	- Phase modulation (Mainly used for Digital Communications)
		- $\phi(t)$ varies linearly with m(t)
- Amplitude modulation
	- $$s(t) = m(t) \cos(2\pi f_c t)$$
	- Modulator: A circuit for generating the modulated signal
		- We don't worry about the analog side of building this circuit, that's for RF circuits (i think). We think of it at a higher level and just use a diagram (Image from class)
	- Mixer: A device that multiplies two signals especially if one of the signals is a carrier![[Pasted image 20261018213428.png]]
- E.g. Digital information (1s 0s) where amplitude $_i_ \in \pm 1$ $$m(t) = \sum_i A_i P_T(t-iT)$$![[Pasted image 20261018215039.png]]
- ![[Pasted image 20261018215238.png]]
![[Pasted image 20261018215247.png]]
	- This looks like the iain explains vid i watched yesterday, so this is pretty cool to see the ji-minator explain it properly
	- some discontinuities :think:
	- This is called Binary (1s 0s) Amplitude-shift keying (ASK)
	- The individual phases of each bit is called a Binary(1s 0s) Phase-Shift Keying (PSK).
- Now let's look in the frequency domain
	- ![[Pasted image 20261018222026.png]]
	- This is called Double Sideband Modulation (DSB) and the carrier signal is not transmitted, so this is called Double Sideband-Suppressed-Carrier (DSB-SC) . Sometimes we do want it like in the lab
