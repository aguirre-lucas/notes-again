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