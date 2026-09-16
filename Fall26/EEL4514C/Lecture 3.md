# Recap
- Consider a sinusoidal EM wave propagating from a source. At the source, the wave is $$s(t, d=0) = A cos(\omega t)$$
- Here $\omega$ is the frequency in radians/s
- Recall that we commonly specify frequencies in Hz
- If $f$ is the frequency in Hz, then $\omega = 2 \pi f$ and $$s(t, d=0) = Acos(2 \pi f t$$
- At some other distance $d$, the wave will be delayed by the propagation time to reach that point $$s(t, d) = A cos (\omega [t - \tau (d)])$$
- The speed of the wave is $c = 3 \times 10^8$ , so $d = c\tau$
- The delay to reach d is $\tau (d) = d/c$
- Thus, at $d$, $$s(t, d) = Acos(\omega t - \omega \frac{d}{c})$$
- The wavelength of the EM wave is the distance spanned by one period of the wave
- Thus, to be one wavelength from the radiator, $$\frac{\omega d}{c} = 2 \pi$$
- Solving this equation will give us the distance in meters/rad, which is not intuitive
- If we use Herts, the result is in meters: $$\begin{align} \frac{2\pi f d}{c} &= 2 \pi \\ \frac{fd}{c} &= 1 \\ d &= \frac{c}{f} & \frac{m/s}{1/s} \\ d &= \frac{c}{f} & m \end{align}$$
- We use $\lambda$ to represent the wavelength in meters
	- Examples
	- ![[Pasted image 20260913215651.png]]
-  RF signals propagate directly out from a source unless they encounter objects in the environment. The way that an RF signal interacts with the surfaces in the environment depends on the relative sizes of the wavelength of the signal and the features of the surface
	1. **Reflection** occurs when the surface is "smooth": features of the surface are large compared to the wavelength 
	2. **Diffraction** occurs when an EM wave impinges on an object that has a "sharp" features
	3. **Scattering** occurs when the surface is "rough": the features of the surface are all small compared to the EM wavelength
* Once transmitted, EM waves propagate without limit and spread out
* *How does the received power vary with distance?*
* *How does the received power vary with frequency?*
* An isotropic radiator is a theoretical antenna that radiates waves in all directions from a single point, also known as an isoradiator or isotrope
# Lecture Agenda
1. Antennas and Propagation
2. Decibel Notation
3. Communication Loss
4. Decibel Notation for Power
# Antennas and Propagation
- The surface of a sphere of radius $d$ is $4 \pi d^2$. So, if $Pt$ watts leave an isoradiator, then the power density at distance $d$ is $\frac{Pt}{4\pi d^2}$
	- Isoradiators waste a lot of power, antennas usually have direction
- The observer is sufficiently far from the transmitter. The antenna has a power gain $Gt$. The far-field power density is $\frac{PtGt}{4\pi d^2}$
- Receive power at distance $d$ $$\begin{align} P_r(d) &= \frac{PtGtAe}{4 \pi d^2} \\ &= \frac{Pt \cdot Gt \cdot Gr  \cdot \lambda^2}{(4 \pi d)^2} \\ &= \frac{Pt \cdot Gt \cdot Gr  \cdot c^2}{(4 \pi df)^2}\end{align}$$ This equation is called the **Friis Free Space Equation**
	- $Ae$ is the effective aperture, $Ae = \frac{\lambda^2 Gr}{4 \pi}$
	- $Gr$ is the receive antenna gain
	- In practice, this ratio will be higher than 2 - ji
- *How does the received power vary with distance?*
	- Power loss $\propto 1 / d^2$; higher distance = less power
- *How does the received power vary with frequency?*
	- Power drop $\propto 1 / f^2$ higher frequency = less power we receive
# Decibel Notation
- $G_{dB} = 10log_{10}G$
	- $G$ is the antenna gain
- To convert from dB to linear units
	- $G = 10^{\frac{G_{dB}}{10}}$
- Common Decibel Values

| Value | Linear | Decibel |
| ----- | ------ | ------- |
|       | 2      | 3dB     |
|       | 5      | 7dB     |
|       | 10     | 10dB    |
* Gains are given in dB w.r.t an isoradiator ($dB_i$) or w/ respect to a half-wave dipole ($dB_d$)
* Note that dB, dBi, and dBd do not indicate units, we say they are dimensionless and indicate an alternative mathematical representation of numbers that also do not carry units. 

| Antenna                 | Gain ($dB_i$) | Gain ($dB_d$) |
| ----------------------- | ------------- | ------------- |
| Half-wave dipole        | 2.15 dB       | 0 dB          |
| Vertical whip           | 1.85 dB       | -0.3 dB       |
| 3-element Yagi          | 4-8dB         | 2-6dB         |
| 1 m dish (at 2-11 GHz)  | 24-39 dB      | 22-37dB       |
| 10 m dish (at 2-11 GHz) | 44-59 dB      | 42-47 dB      |
- E.g. the transmit antenna gain from the DSS43 (antenna used on voyager) in the x-band (7-11GHz) for Voyager 2 space craft is 62.1 dBi. Determine the gain, Gt, in linear units.
	- $$ gT = 10^{\frac{62.1}{10}} \approx 1.6 \times 10^6$$ (Something that's done in the homeworks)
# Communication Loss
- Communication loss represents the signal attenuation as a positive quality in dB $$PL_{dB} = 10 log_{10} \left ( \frac{Pt}{Pr} \right )$$
- Communication loss may include or exclude antenna gains
	- with antenna gains $$\begin{align} PL_{dB} &= 10 log_{10} \left ( \frac{Pt}{\frac{Pt \cdot Gt \cdot Gr  \cdot c^2}{(4 \pi df)^2}} \right ) \\ PL_{dB} &= - 10 log_{10} \left ( \frac{GtGr\lambda^2}{(4 \pi d)^2} \right ) \end{align}$$ Cancel out the Pt's and you get the equation including the antenna gains from the Friis Free Space equation
	- without antenna gains $$\begin{align} PL_{dB} &= -10 log_{10} \left ( \frac{\lambda^2}{(4 \pi d)^2} \right ) \\ PL_{dB} &= -10 log_{10} \left ( \frac{(\frac{c}{f})^2}{(4 \pi d)^2} \right ) \\ PL_{dB} &= 20 log_{10}(f) + 20log_{10}(d) - 20log_{10}(\frac{c}{4\pi}) \\ PL_{dB} &= 20 log_{10}(f) + 20log_{10}(d) - 147.6dB\end{align} $$ Notice that 20log10(c/4pi) is a constant that approximates to 147.6dB
	- In practice that ^2 on the denominator in the beginning is called the "loss exponent" and could be bigger than 2; say n=4, instead of 20log10d it'd be 40log10d. Useful formula that can be used in many cases  