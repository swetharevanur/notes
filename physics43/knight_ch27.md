# Knight Physics, Ch. 27: Current and Resistance (written referencing [this](http://faculty.uml.edu/arthur_mittler/Teaching/95_144/Ch%2027%20Knight%204th.pdf))

## Key Formulas and Ideas
- Current represents the flow of positive charge.
- Conservation of charge from Kirchhoff's junction law.
 
![](https://image.ibb.co/eLGY3y/Screen_Shot_2018_05_14_at_2_54_03_PM.png)

- The electric field causes a drift of the sea of free-moving conducting electrons (opposite direction of current). More electron collisions means higher resistivity and lower conductivity.
- The drift speed is $v_d = \frac{e\tau}{m}E$, where $\tau$ is the mean time between collisions.
- The electron current is related to the drift speed by $i_e = n_eAv_d$ where $n_e$ is the electron density.
- Conductivity is $\sigma = \frac{n_ee^2\tau}{m}$ and resisitivity is $\rho = 1/\sigma$.
- The potential difference $\Delta V_{wire}$ between the ends of a wire creates an electric field inside the wire: $$E_{wire} = \frac{\Delta V_{wire}}{L}$$
- The electric field causes a current in the direction of decreasing potential. From Ohm's law size of the current is $$I = \frac{\Delta V_{wire}}{R}$$ where $R = \frac{\rho L}{A}$ is the wire's resistance.

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
- Conductivity is another property of a material. The formula is: $$\sigma = \frac{n_ee^2\tau}{m}$$
- $J = \sigma E = I/A$
- **Resistivity** (another material property) tell us how reluctantly the electrons move in response to an electric field: $$\rho = \frac{1}{\sigma} = \frac{m}{n_ee^2\tau}$$

### Superconductivity
- Sudden and complete loss of all resistance to current at low temperatures is known as **superconductivity**.
- Superconductors have unusual magnetic properties.

## Ch. 27.5: Resistance and Ohm's Law
- Current is related to voltage by $I = \frac{A}{\rho L}\Delta V$.
- The current through a conductor is inversely proportional to the potential different between its end.
- **Resistance** $R$ of a long, thin conductor of length $L$ and cross-sectional area $A$ is $$R = \frac{\rho L}{A}$$
- Measured in ohms; $1 \Omega = 1 V/A$.
- **Ohm's Law** states that the current through a conductor is determind by the potential difference $\Delta V$ along its length: $$I = \frac{\Delta V}{R}$$.
- Ohm's law only applies to *ohmic* materials whose $R$ is constant.
- Metals and other conductors are ohmic devices.
- When the current through the device is not directly proportional to the potential difference, those materials are nonohmic (e.g. diodes, batteries, and capacitors).
- In a **battery-wire-resistor-wire circuit, the current $I$ through the resistor is the same as the current in each wire.**

![](https://image.ibb.co/fgcvGJ/Screen_Shot_2018_05_14_at_2_47_57_PM.png)