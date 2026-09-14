> Day 1 was introductions, didn't make it because I was sick. 
> Doesn't really have slides, will bring paper next time
- This class focuses on continuous-time signals and systems, and serves as an application of them and how signals and systems work in wireless communications
# Lecture 02
Agenda:
1. Wavelength of a sinusoid
2. effects of surface propagation (emg signals touching surfaces give feedback)
3. Antennas and propagation

Speed of light: $c = 3*10^8$
SI properties: $k=10^3, m=10^6, g=10^9, t=10^12$
## Wavelengths of a Sinusoid
- Consider a sinusoid electromagnetic (EM) wave propagating from a source. 
	- ![[Pasted image 20260913191337.png]]
	- $\Delta$ <-- mobile station : sin(t) graph. One period = One Wavelength
	- **The wavelength of the EM wave is the distance spanned by one period of the wave.** This period is known as the *fundamental period* of the sinusoid. 
- At some distance *l*, the wave will be delayed by the propagation time to reach that point.
	- Transmitted Signal: $s(t, d) = Acos(\omega\left (t - \tau \left (d \right ) \right ))$
		- we always transmit cos(), just accept it for now it'll be explained later.
	- The speed of the wave is *c*, so *d* =  $\tau \cdot c \Rightarrow$ the delay to reach d; $\tau(d) = \frac{d}{c} \Rightarrow s(t,d) = Acos(\omega t - \omega \frac{d}{c})$
	- $\Rightarrow \frac{wd}{c} = 2\pi \Rightarrow d = \frac{2\pi c}{\omega}$
	- The fundamental period of a cos is 2pi, so we know the length of one period is 2pi, allowing us to let wd/c = 2pi. This also makes the units meters
- IF we use Hertz, then 
	- $\omega = 2\pi f$
	- $d = \frac{2\pi c}{2\pi f} = \frac{c}{f} = \lambda$ <-- Remember this!
	- $\lambda$ : used to represent the wavelength in hertz, represented in meters. Unit conversion nonsense removes the s^2 because f=1/s
- Example:
	- Cellular, if we're operating at 900 MHz, compute the wavelength
		- $\lambda = \frac{c}{f} = \frac{3*10^8}{9*10^8} = \frac{1}{3}$ m
	- AM Radio: around 1000kHz
		- $\lambda = \frac{c}{f} = \frac{3*10^8}{10^6} = 300$ m
##  Effects of surfaces on propagation

- RF Signals (air signal) propagates directly out from a source unless they encounter objects in the environment
  ![[Pasted image 20260913200717.png]]
- The way that RF signals interact with surfaces in the environment depends on the relative surface of the wavelength of the signals and the features of the surface
- **Reflection** occurs when the surface is smooth: features of the surface are *large* compared to the wavelength. 
		- ![[Pasted image 20260913201322.png]]
- **Diffraction** occurs when the EM signals infringes on an object that has "sharp" feature
		- ![[Pasted image 20260913201600.png]]
- **Scattering** occurs when the surface is "rough": features of the surface are *small* compared to the wavelength
	- ![[Pasted image 20260913201726.png]]
## Antennas and Propagation
* How does the received power vary with distance?
* How does the received power vary with frequency? ($\lambda = \frac cf$)
* An Isotropic Radiator is a theoretical antenna that radiates waves in all directions from a single point (isoradiator of isotrope) 
	* pebble in a pond, all waves move from a single point
	* ![[Pasted image 20260913205258.png]]
* 
