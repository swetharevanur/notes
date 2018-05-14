# Knight Physics, Ch. 25: The Electric Potential (written referencing [this](http://faculty.uml.edu/arthur_mittler/Teaching/95_144/Ch%2025%20Knight%204th.pdf))

## Key Formulas and Ideas
- Conservation of energy!
- $\Delta U = -W_{interaction}$
- General expression for electric potential energy of a system with two point charges is $$U_{elec} = \frac{Kq_1q_2}{r}$$
- $U_{dipole} = -pEcos\theta$
- $V = \frac{U_{q + sources}}{q}$
- The electric potential inside a PPC is $V = Es$, where $s$ is the distance from the negative electrode.
- Given $\Delta V$ and $d$, the electric field strength within a PPC is $E = \frac{\Delta V}{d}$.
- The potential energy due to source point charge $q$ is $U_{sourceCharge + probeCharge} = \frac{kqq'}{r}$ if they are distance $r$ apart.
- So the electric potential of a point charge (and charged sphere) is $$V = \frac{U_{q'+q}}{q'} = \frac{kq}{r}$$
- Superposition applies for potential.
- **Potential is a scalar!** No angles needed to compute - just sum up.
- Units:
	- Potential: $1 V = 1 J/C$
	- Electric field: $1 V/m = 1 N/C$

![](https://image.ibb.co/j8cyJy/Screen_Shot_2018_05_14_at_1_20_24_AM.png)

- [Intuition behind voltage](https://www.quora.com/Intuition-of-voltage-in-electric-field): voltage is the "height" in the electric field instead of the gravitational one. $U = Vq$ is analogous to gravitational potential energy. Your voltage times charge says how much energy you'd gain "falling" in the electric field a certain distance, and it's scaled by your charge. It takes into account "height" but for the electric force. The "higher" you go, in gravitational or electric fields, the more potential energy you have.

## Ch. 25.1: Electric Potential Energy

### Mechanics Review
- $E_{mech} = K + U$ conserved for particles that interact via conservative forces.
	- $K$ is the system's energy of motion. $U$ is the system's interaction energy.
	- **Work** is done when a force acts on a particle as it's being displaced. $W = F\Delta r cos\theta$ when the force is constant. Alternatively, if force is not constant, you can divide the path into small segments of length $dx$ and integrate over them such that $W = \int_{x_i}^{x_f}F(x)cos\theta dx$.
	- If you drop a book, the book has done positive work. However, in falling, the book has lost potential energy. So, $\Delta U = -W_{interaction}$.
	- Conservative force = work independent of path take. **Electric force is conservative**, and so there is **electric potential energy**.

### Uniform Electric Field
- A charged particle inside a parallel-plate capacitor with electrode spacing $d$ is analogous to a particle in a gravitational field dropping from height $d$.

![](https://image.ibb.co/h1SQFd/Screen_Shot_2018_05_13_at_3_10_29_PM.png)

- The positive charge $q$ inside the capacitor speeds up and gains kinetic energy and **loses electric potential energy** as it "falls" towards the negative plate.
- The electric potential energy of $q$ in a uniform electric field is $U_{elec} = U_0 + qEs$, where $s$ is the distance from the negative plate and $U_0$ is the potential energy there. True regardless of $q$'s sign!
- **Problem Type:** Charged particle acceleration in a parallel plate capacitor (or other uniform electric field)
	- **Strategy:** Use conservation of energy and solve for $v_f$.

## Ch. 25.2: The Potential Energy of Point Charges

- General expression for electric potential energy of a system with two point charges is $$U_{elec} = \frac{kq_1q_2}{r}$$
- Also the electric potential energy of two charged spheres where $r$ is the distance between their centers.

### Escape Speeds
- Two oppositely charged particles can escape from each other if $E_{mech} > 0$. The threshold condition is $E_{mech} = 0$, which allows the particles to reach infinite separation at infinitely slow speed. 
- **Problem Type:** Escape speed of two interacting particles
	- **Strategy:** Use conservation of energy. When particles end up far apart, $U_f \approx 0$.

### Electric Force is Conservative
- Again, this means work done is independent of path.

### Multiple Point Charges
- If multiple charges are present, their potential energy is the sum of the potential energy due to **all pairs of charges**: $$U_{elec} = \sum_{i<j}\frac{kq_iq_j}{r_{ij}}$$
- For energy conservation problems, only calculate $U$ for pairs of charges for which distance $r_{ij}$ changes.
- Unstable equilibrium = when little changes will create forces that moves the particle.

## Ch. 25.3: The Potential Energy of a Dipole
- When a dipole is an uniform electric field, forces exert a torque on it. The work done is $$W_{elec} = pEcos\theta_f - pEcos\theta_i$$
- The potential energy of a dipole is minimum at $\theta = 0$, when the dipole is aligned with the electric field. This leads to a stable equilibrium.
- $U_{dipole} = -pEcos\theta$
- When a dipole is at the point of minimum energy, it won't spontaneously move - an external force needs to be applied.

## Ch. 25.4: The Electric Potential
- The electric potential (aka voltage) is $V = \frac{U_{q + sources}}{q}$.
- Units: 1 volt = 1 $V$ = 1 $J/C$.
- A battery is a source of electric potential because the + and - ends have some potential difference.
- Test charge + source charges. The test charge is a probe to determine the electric potential which is actually caused by source charges.
- You can still use conservation of energy here: $$K_f + qV_f = K_i + qV_i$$
- Relationship between electric potential and speed (kinetic energy):
![](https://preview.ibb.co/k2wayy/Screen_Shot_2018_05_14_at_12_26_38_AM.png)
- Typically, if you do conservation of energy problems and find the final speed of a proton, and then switch out the proton with an electron, the electron will have a higher final speed (due to equal/opposite charge but smaller mass).

## Ch 25.5: The Electric Potential Inside a Parallel-Plate Capacitor (PPC)
- The electric field inside a PPC is $E = \frac{\eta}{\epsilon_0}$ from positive to negative, where $\eta$ is the charge density.
- The **electric potential inside a PPC** is $V = Es$, where $s$ is the distance from the negative electrode. The potential difference ($\Delta V$) between two capacitor plates $d$ apart is thus $Ed$.
- Given $\Delta V$ and $d$, the **electric field strength within a PPC** is just $E = \frac{\Delta V}{d}$.
	- Demonstrates new units for electric field! $1 N/C = 1 V/m$.
- **Equipotential surface:** any surface with the same electric potential at every point. **In a PPC, $V$ increases linearly with $s$ (distance from negative electrode).**
- The electric field 
	- **points *perpendicularly* to the equipotential surfaces.**
	- **points in the direction of *decreasing* potential**
![](https://image.ibb.co/mwxJkd/Screen_Shot_2018_05_14_at_12_42_22_AM.png)
- Doesn't matter where you let $V = 0$ because potential difference between PPC electrodes is the same and electric field inside is the same too.

## Ch. 25.6: The Electric Potential of a Point Charge
- The potential energy due to source point charge $q$ is $U_{sourceCharge + probeCharge} = \frac{kqq'}{r}$ if they are distance $r$ apart.
- So the electric potential of a point charge is $$V = \frac{U_{q'+q}}{q'} = \frac{kq}{r}$$
- Potential extends through space, inversely linearly with $r$. Here, the equipotential surface is the sphere of radius $r$ around the point charge.

### Electric Potential of a Charged Sphere
- Outside of radius $R$ sphere, electric potential is just as if you had point charge $Q$ at the center:
$V = kQ/r$ where $r \geq R$.
- If you have the potential $V_0$ on the surface, you can multiply by $R/r$ to get potential at distance $r \geq R$.

## Ch. 25.7: The Electric Potential of Many Charges
- Sum up potentials due to each charge; **superposition is obeyed**. Divide total charge up into point charges and sum potentials when asked to compute electric potential of a continuous charge distribution.

### Example #1: electric potential of an electric dipole
- Models a human heart
![](https://image.ibb.co/c2Acdy/Screen_Shot_2018_05_14_at_1_08_49_AM.png)

### Example #2: potential of a ring of charge about an axis
- Divide up thin ring into small segments:
![](https://image.ibb.co/kb3zQd/Screen_Shot_2018_05_14_at_1_14_58_AM.png)
- Using superposition:
$$V = \sum_{i=1}^N V_i = \sum_{i=1}^N \frac{k\Delta Q}{r_i} = \frac{k}{\sqrt{R^2 + z^2}}\sum_{i=1}^N\Delta Q = \frac{kQ}{\sqrt{R^2 + z^2}}$$
- When $z >> R$, $\sqrt{R^2 + z^2} \to z$ and it's as if the ring is a point charge.