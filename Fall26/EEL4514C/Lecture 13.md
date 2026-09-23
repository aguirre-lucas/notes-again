# Agenda
1. Power Spectra Density
2. Modulation
# Power Spectra Density
- Special case: Random Power Signal
	- $$R(\tau) = \lim\limits_{T\rightarrow \infty}\frac1T\int_{-T/2}^{T/2}g(t)g^*(t-\tau)dt$$Something with Ergodicity
# Modulation
- General definition: **The mapping of information to a signal that is appropriate for transmission over channel**
- Carrier Modulation   
	- What is a carrier? In this class, it's a sinusoid signal (cos()) used to map the PSD of a message signal to a different frequency band
	- The frequency of a carrier is called the carrier frequency $f_c$ , sometimes $f_0$
	- The simplest form of the carrier modulation is $$s(t) = m(t)\cos(2\pi f_ct)$$Where s(t) is the modulated signal, m(t) is the message signal, and the cos() is the carrier. \[Image from class\]
	- (Image from class) A type of amplitude modulation
	- $$\begin{align} s(t) &= m(f) * \frac12(\delta(f-f_c) + \delta(f+f_c)) \end{align}$$(Image from class) $$s(t) = A(t) \cos \right ( \left )$$
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
	- Mixer: A device that multiplies two signals especially if one of the signals is a carrier signal
- E.g. Digital information (1s 0s) where amplitude $A \in \pm 1$ $$m(t) = \sum_i A_i P_t(t-iT)$$(Drawing from class of waveform Pt())
- (Drawing from class of the multiplication to the square wave (digital signal))
	- This looks like the iain explains vid i watched yesterday, so this is pretty cool to see the ji-minator explain it properly
	- some discontinuities :think:
	- This is called Binary (1s 0s) Amplitude-shift keying (ASK)
	- The individual phases of each bit is called a Binary(1s 0s) Phase-shit keying (PSK).