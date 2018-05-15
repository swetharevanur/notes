# Knight Physics, Ch. 28: Fundamentals of Circuits (written referencing [this](http://faculty.uml.edu/arthur_mittler/Teaching/95_144/Ch%2028%20Knight%204th.pdf))

## Ch. 28.1: Circuit Elements and Diagrams
- **Circuits** = controlled motion of charges through conductors and resistors.
- **DC circuits** = direct current; potentials and currents are constant.

### Circuit Diagrams
- In the image below, we see a figure of a resistor and capacitor connected to wires:

![](https://image.ibb.co/bv7F0d/Screen_Shot_2018_05_14_at_5_47_49_PM.png)

- Here are the building blocks of a circuit diagram:

![](https://image.ibb.co/dRRQ0d/Screen_Shot_2018_05_14_at_5_49_42_PM.png)

## Ch. 28.2: Kirchhoff's Laws and the Basic Circuit
- You can analyze any circuit with Kirchhoff's two laws:

1. Junction law (charge conservation) relates the currents at a junction; $\sum I_{in} = \sum I_{out}$.
2. Loop law (energy conservation) relates the voltage around a closed loop; $\Delta V_{loop} = \sum (\Delta V)_i = 0$.
3. Extra: Use Ohm's law for resistors.

- Example using loop law:

![](https://image.ibb.co/m4qzty/Screen_Shot_2018_05_14_at_5_56_22_PM.png)

- The most basic circuit is a resistor connected to the two terminals of a battery. This is a complete circuit (continuous path).
- When you have two lightbulbs in series they produce light by using the potential energy created by the battery's potential difference.

## Ch. 28.3: Energy and Power
- The power dissipated by a resistor is $P_R = I\Delta V_R = I^2R = \frac{(\Delta V_R)^2}{R}$.
- The electric field causes electrons to speed up (energy goes from potential to kinetic). A resistor dissipates power because the electric force does work on the charges.
- The product of watts and seconds is joules (unit of energy). But most energy companies use the kilowatt hour to measure monthly energy usage.

## Ch. 28.4: Series Resistors
- Lightbulbs are considered resistors. When multiple bulbs are placed in series, less current flows through than if there was just one bulb. So the one bulb circuit will be brighter than either of the bulbs in a two bulb circuit.
- This is called **series resistors**. $\Delta V_{total} = IR_1 + IR_2 = I(R_1 + R_2)$.
- The current $I$ through all resistors is the same. **Equivalent esistance is additive for multiple resistors in series!** You can replace two series resistors with an equivalent resistor with no consequence.

### Ammeters
- Device that measures the current in a circuit element. Must be placed in series with whatever it's trying to measure.
- Resistance of ammeter is negligible.
- Must break connections and then add ammeter in series.

## Ch. 28.5: Real Batteries
- Real batteries have an internal resistance $r$. This internal resistance can be physically separated, but it's still in series with the battery.
- **Short circuit:** when a connection of very low or zero resistance is made between two points in a circuit that are normally separated by a higher resistance. 
- An ideal battery can supply infinite current when shorted.

## Ch. 28.6: Parallel Resistors
- When you close a switch in a circuit, an alternate pathway for the current is created. This decreases overall resistance. You can think of this alternate pathway as a junction and **apply Kirchhoff's junction law**.
- Potential difference is the same for all resistors in parallel.
- An equivalent resistor for two in parallel would be $$R_{eq} = (\frac{1}{R_1} + \frac{1}{R_2} + ...)^{-1}$$
- True definition of parallel resistors: those connected at both ends.

### Voltmeters
- Measures potential difference across a circuit element. Must be placed in parallel with whatever it's trying to measure.
- Has very high resistance
- No need to break connections like in an ammeter.
- To find internal resistance: $$r = \frac{\epsilon - \Delta V_R}{\Delta V_R}R$$
where $\Delta V_R$ is the potential difference measured by the voltmeter.

## Ch. 28.7: Resistor Circuits
- When solving resistor circuit problems, reduce the circuit to the smallest possible number of equivalent resistors.
- Write Kirchhoff's loop law for each independent loop in the circuit. This law applies to any loop, and even multi-loop systems.
- Determine the current through and potential difference across the equivalent resistors.
- Finally, rebuild the circuit, using the facts that **the current is the same through all resistors in series and the potential difference is the same for all parallel resistors.**

## Ch. 28.8: Getting Grounded
- Wire connecting circuit to the eath is not part of a complete circuit. So, there's no current! In this case, the circuit is said to be **grounded**.
- Potential at the ground is $V_{ground} = 0$.
- Grounding the circuit allows us to have specific values for potential at each point in the circuit, rather than just potential differences. 
- Changing the ground point does not affect the circuit's behavior.

## Ch. 28.9: RC Circuits
- An RC circuit has both resistors and capacitors.
- The resistor current is the rate at which charge is removed from the capacitor: $$I = -\frac{dQ}{dt}$$
- If you integrate over time, $Q = Q_0 e^{-t/\tau}$ where $\tau = RC$. Same decay equation for $I$ and $\Delta V$.
- Over time, charge and current both decay exponentially in RC circuits.
- With an RC circuit, you can both charge and discharge a capacitor. We can model the capacitor charging with "upside-down decay": $Q = Q_0(1 - e^{-t/\tau})$.