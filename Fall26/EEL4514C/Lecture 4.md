Not writing the recaps anymore, just go back in the notes to look at it
# Agenda
1. Communication Loss
2. Decibel Notation for Power
3. Review signals & systems
# Communication Loss
* Relative power and path loss
* Large-scale propagation models often use some close-in distance $d_0$ as a known received power reference point.
* $P_r(d_0)$ can obtained by measure or Friis free space equation
	* then $$\begin{align} P_r(d_0) &= P_r(d) \cdot \left ( \frac{d^2}{d_0^2} \right ) \\ [P_r(d)]_{dB} &= 10log_{10} \left [P_r(d_0) \cdot \left ( \frac{d^2}{d_0^2} \right ) \right ] \\ [P_r(d)]_{dB} &= 10log_{10} [ P_r(d_0) ]  + 10log_{10} \left ( \frac{d^2}{d_0^2} \right ) \\  [P_r(d_0)]_{dB} &= [P_r(d_0)]_{dB} - [P_l]_{dB} \end{align} $$ Note 10log10(pr(d0) became the left term and the other became the right term'
* A factor of 2 is equal to 3dB, therefore every factor of 2 in distance results in an additional 6dB path loss
* e.g. Voyager 2 has left the Sun's heliosphere and is approximately 19.5 billion km from Earth. Find the path loss over that distance in dB
	* $$\begin{align} [P_l]_{dB} &= 20\log_{10} \left ( \frac{19.5 \times 10^{12}}{1} \right )  \\ &\approx 266 \space dB\end{align}$$
	* We use 1 as a reference distance $d_0$
	* Linear scale: $$\begin{align}  P_l &= 10^{\frac{266}{10}} \\ &\approx 3.8 \times 10^{26} \approx \left ( \frac{19.5 \times 10^{12}}{1}\right )^2 \end{align}$$
# Decibel Notation for Power
- Linear units: Watts, mW
	- $P_W$: power in Watts
	- $P_{mW}$: power in milliwatts
		- $P_{mW} = 1000 P_{W}$
	- $P_{dBW} = 10\log_{10} P_W$
		- Power in decibel-watts (dBW)
	- $P_{dBm} = 10\log_{10} P_{mW}$
		- Power in decibel-milliwatts
		- $$\begin{align} P_{dBm} &= 10\log_{10}[ P_mW ] \\ &= 10\log_{10} [1000 P_W] \\ &= 10\log_{10}[P_W] + 10\log_{10}[1000] \\ &= \boxed{P_{dBW} + 30}\end{align}$$
- e.g. A CDMA (co-division multiple access 3G standard not in use anymore) base station operating at 1897 MHz uses approximately 13dBW transmit power for each user's channel. The power is transmitted through a directional antenna that has a 17dBi gain. The receive antenna gain is approximately 3dBi. What is the received power at 100m? 
	- Use Friis Free Space Equation to compute it
	- $$\begin{align} P_r(d) &= \frac{Pt \cdot Gt \cdot Gr  \cdot c^2}{(4 \pi df)^2} \\ P_t &= 13_{dBW} = 10^{\frac{13}{10}} &= 20W \\ G_t &= 17_{dBi} = 10^{\frac{17}{10}} &= 50 \\  G_r = 3_{dBi} = 2 \\ P_r(100) &= \frac{20 \times 50 \times 2 \times (3 \times 10^8)^2}{(4\pi(100)(1897 \times 10^6))^2} \\ P_r(100) &\approx 3.2 \times 10^{-5} \\ [P_r]_{dBW} &= 10\log_{10}[3.2 \times 10^{-5}] &=  -45dBW = =15dBm \end{align}$$