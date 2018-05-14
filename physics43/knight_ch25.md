# Knight Physics, Ch. 25: The Electric Potential

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

## Ch. 25.2: The Potential Energy of Point Charges

- General expression for electric potential energy of a system with two point charges is $$U_{elec} = \frac{Kq_1q_2}{r}$$
- Also the electric potential energy of two charged spheres where $r$ is the distance between their centers.

### Escape Speeds
- Two oppositely charged particles can escape from each other if $E_{mech} > 0$. The threshold condition is $E_{mech} = 0$, which allows the particles to reach infinite separation at infinitely slow speed. 

### Electric Force is Conservative
- Again, this means work done is independent of path.

### Multiple Point Charges
- If multiple charges are present, their potential energy is the sum of the potential energy due to **all pairs of charges**: $$U_{elec} = \sum_{i<j}\frac{Kq_iq_j}{r_{ij}}$$
- For energy conservation problems, only calculate $U$ for pairs of charges for which distance $r_{ij}$ changes.