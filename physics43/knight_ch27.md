# Knight Physics, Ch. 27: Current and Resistance (written referencing [this](http://faculty.uml.edu/arthur_mittler/Teaching/95_144/Ch%2027%20Knight%204th.pdf))

## Key Formulas and Ideas

## Ch. 27.1: The Electron Current
- To discharge a capacitor (that would otherwise stay charged indefinitely), connect the plates to a wire. As the capacitor discharges, there is current in the wire.
- When there is current, **conductors are *not* in equilibrium.**
- Valence electrons in the sea are the charge carriers in metals.
- Electon current $i_e$ = # electrons that pass through cross-section of conductor every second at drift speed $v_d$. 
- The electron current is the electron density ($n_e$) times the cross-section area ($A$) times the drift speed ($v_d$)
- In most most metals, one atom contributes one valence electron to the sea of electrons. Thus the number of electrons $n_e$ is the same as the number of atoms per cubic meter.
- Even though the sea of electrons moves slowly, discharging a capacitor happens quickly. This is because the discharging wire is already full of electrons and so the charges on the plates move the electrons just enough to neutralize the plates.

## Ch. 27.2: Creating a Current
- The sea of electrons will slow down and stop unless there's a force acting on it (like an electric field)!
- Say you have two wires, each connected to a plate (positive or negative). Before you connect the two wires, there is no current and no electric field in the wires. When you connect the two wires, the surface rearranged into a nonuniform distribution.

![](https://image.ibb.co/jgiXDy/Screen_Shot_2018_05_14_at_1_35_28_PM.png)

- **This nonuniform distribution creates a net electric field inside the wire that points from positive end to negative end.**

![](https://image.ibb.co/ka9Hfd/Screen_Shot_2018_05_14_at_1_41_09_PM.png)

### Model of Conduction
- When a conductor is in equilibrium and there's no electric field, an electron has frequent collisions but no net displacement and no average velocity.
- When there is an electric field, the force causes **electrons to move along parabolic trajectories between collisions in the direction opposite the field**. Therefore, the sea of electrons experiences net motion in the direction opposite the field.
- The average drift speed of the sea if $v_d = \frac{e\tau}{m}E$, where $\tau$ is the mean time between collisions.
- We can substitute this expression for $v_d$ into the expression for electron current and get: $$i_e = n_eAv_d = \frac{n_ee\tau A}{m}E$$
- $n_e$ and $\tau$ are properties of the metal.
- The average number of collisions per second is the inverse of $\tau$.

## Ch. 27.3: Current and Current Density
- **Current $I$ is the flow of positive charge** through a conductor. Can't see it, but there are other indicators like compass needle moves or a wire with current gets warm.
- Measured in **amperes** ($1 A = 1 C/s$), which is charge flow rate of one coulomb per second.
- $I = \frac{dQ}{dt} = ei_e$
- Since current if the flow of positive charge, it moves in the opposite direction of $i_e$, which is the motion of actual charge carriers (i.e. in the same direction as the electric field).

### Current Density
- **Current density** $J$ in a wire = current per square meter of cross section = $1/A = n_eev_d$.
- Measured in units of $A/m^2$.

- **Current is conserved.** Rate of electrons leaving some device is the same as the rate of electrons entering it.

### Kirchhoff's Junction Law
- In a wire without junctions, the current is the same at all points. This is due to conservation of charge.
- If there is a junction, $\sum I_{in} = \sum I_{out}$.

## Ch. 27.4: Conductivity and Resistivity

## Ch. 27.5: Resistance and Ohm's Law