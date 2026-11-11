# Agenda
1. Angle Modulation
	1. Intro
	2. Properties of Angle Modulation
# Introduction
- Motivation: We've been talking about a lot of types of amplitude modulations recently, but there's a problem where coherent/noncoherent detection, demodulation design might not be easy, filtering design might be complex, not power efficient sometimes, but there's another big issue. In amplitude modulation, the envelope represents the information, but there's a disadvantage to where the amplitude modulation is genuinely just noisy. **In amplitude modulation the output signal is directly proportional to the input signal, therefore any noise or interference in the received signal will directly appear in the output signal**
- Let's solve this using angle modulation so that the noise doesn't directly affect the signal
- Let $\theta(t)$ be the angle (phase) of a modulated carrier, then an angle-modulated signal can be written as $$S_<(t) = A \cos[\theta_t]$$
- Relationship between freq and phase
	- $s(t) = \cos(\omega_ct)$
	- $\theta(t) = \omega t$
	- frequency = $\omega_c$
	- $\omega_c = \frac{d\theta(t)}{dt}$ 
		- Frequency is the derivative of phase
- Let $w_i(t)$ be the instantaneous frequency at time t $$\omega_i(t) = \frac{d\theta(t)}{dt}$$
- Two basic types of angle modulation
	1. Phase modulation (PM)
	2. Frequency Modulation (FM)
## Phase Modulation (PM)
- $\theta(t)$ is varied linearly with m(t)
- $\theta(t) = \omega_c t + k_pm(t)$ where $k_p$ is the phase sensitivity
- unit of $k_p$: radian/V
- then $$S_{PM}(t) = A \cos [ \omega_c t + k_pm(t)] \\ \omega_i(t) = \frac{d\theta(t)}{dt} = w_c + k_p\frac{dm(t)}{dt} \\ f_i(t) = f_c + \frac{k_p}{2\pi} \frac{dm(t)}{dt} $$ We can also denote the derivate of m(t) as m'(t)
## Frequency Modulation (FM)
- In FM, the instantaneous frequency varies linearly with m(t)
- $\omega_i(t) = \omega_c + k_fm(t)$
- Where $k_f$ is the frequency sensitivity
- Unit of $k_f$: rad/v/s
	- In Hertz: $f_i(t) = \frac{\omega_i(t)}{2\pi} = f_c + \frac{k_f}{2\pi} m(t)$
- $\theta(t) = \int_0^t\omega_i(\alpha)d\alpha = \omega_c + k_f\int_0^tm(\alpha)d\alpha$
- Thus $$S_{FM}(t) = A\cos \left [ \omega_c t + k_f \int_0^tm(\alpha)d\alpha \right ]$$
- Generalized Angle Modulation: $$S(t) = A\cos \left [ \omega_ct + \int_0^tm(\alpha)h(t-\alpha)d\alpha\right] \\ = A\cos[\omega_ct + k_fm(t) * h(t)]$$where h(t) is some filter
	- For PM: h(t) = $k_p\delta(t)$
	- For FM: h(t) = $k_f$
# Properties of Angle Modulation
- Power
	- If f_c > B of m(t), on each period of carrier, m(t) $\approx$ constant, so we can consider this signal as a periodic signal. so
	- $$||s_<(t)||^2 = \frac{1}{T_0} \int_{-T_0/2}^{T_0/2}A^2\cos^2(\omega_ct + c)dt = \frac{A^2}{2}$$
	- **Power is a constant**
- Angle Modulation is nonlinear
	- Let's check linearity for DSB-SC![[Pasted image 20261109152310.png]]
		- We can see it's linear
	- Now if we try to PM?![[Pasted image 20261109152330.png]]
		- We can see it's not linear
- Effects of Nonlinearity
	- Our DSB-SC is a linear operator, so if we pass it through our PM system we see it's not linear. Why is that?
	- ![[Pasted image 20261109152745.png]]
	- Evaluating the output out, we get![[Pasted image 20261109153251.png]]which is detrimental to our bandwidth, also there's distortion at the 3/4m^3(t), but we normally filter out the 3B part. 