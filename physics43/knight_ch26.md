# Knight Physics, Ch. 26: Potential and Field (written referencing [this](http://faculty.uml.edu/arthur_mittler/Teaching/95_144/Ch%2026%20Knight%204th.pdf))

## Key Formulas and Ideas
- Electric field is the negative slope of the potential. 
- The sum of all potential differences around a closed path is 0.
- The potential of a battery is the emf $\epsilon$, or the work per charge done by the charge escalator.
- For a conductor in equilibrium, everything we learned before still holds. In addition, the surface is an equipotential and the interior is at the same potential as the surface.
- The capacitance of two conductors charged to $\pm Q$ is $C = Q/\Delta V_C$. Adding a dielectric increases capacitance.
- You can combine capacitors in series or in parallel.

## Ch. 26.1: Connecting Potential and Field
- Field and potential are just two different perspectives of how source charges alter the space around them. 
- Force is to potential energy as field is to electric potential.
- $\Delta V = -\int_{s_i}^{s_f}Eds$ (aka negative area under the $E$ vs. $x$ curve). The minus sign tells us that the potential *decreases* along the field direction.
- If you know one, you can find the other! Specifically, if $E$ is known, use these tactics numbered in this examples:
![](https://image.ibb.co/ibf2vd/Screen_Shot_2018_05_14_at_2_04_56_AM.png)
- The **potential of a PPC is $V(s) = Es$** if $s = 0$ at the negative plate. Linear relationship.

## Ch. 26.2: Finding the Electric Field from the Potential
- To find $E$ from $V$, we can follow these steps (work to potential to field):
 
1. Set up the problem. There are two points $i$ and $f$ separated by distance $\Delta s$.
2. The work done by the electric field as a charge $q$ moves from $i$ to $f$ is $W = F_s\Delta s = qE_s\Delta s$ (because moving in direction of field, so no angle).
3. The potential different between the points is $\Delta V = \frac{\Delta U_{q+sources}}{q} = \frac{-W}{q} = -E_s\Delta s$.
4. The electric field is $E_s = \frac{-\Delta V}{\Delta s}$. As $\Delta s \to 0$, $E_s = \frac{-dV}{ds}$.

- But if you already know the expression for potential, simply define the axis of interest and take the derivative with respect to that. If multiple dimensions, take the partial w.r.t. each dimension.
- Graphically, this means that the electric field is the negative slope of the potential.
- $E$ is perpendicular to equipotential surfaces everywhere. 
- The field strength is inversely proportional to the spacing between the equipotential surfaces.
- Equipotential surfaces have equal potential differences between them.
- If you run gradient descent on $V$ (i.e. keep stepping in the direction of $E$), you will reach point of minimum potential.

- **Kirchhoff's Loop Law:** For any path that starts and ends at the same po
- int, $\Delta V_{loop} = \sum_i(\Delta V)_i = 0$.

## Ch. 26.3: A Conductor in Electrostatic Equilibrium
- Properties of conductors in equilibrium:
![](https://preview.ibb.co/dxbA0d/Screen_Shot_2018_05_14_at_10_30_52_AM.png)
- Since a conductor surface must be an equipotential, the equipotential surfaces close to each electrode roughly match the shape of the electrode.
- If you have multiple equipotential surfaces near each other, the equipotentials surfaces gradually change their shape from one electrode to the other.

## Ch. 26.4: Sources of Electric Potential
- Potential difference is caused by separation of charge (which leads to an electric field and hence the potential difference). This difference can cause sparks.
- Electric forces try to bring opposite charges together, so you need an external, nonelectrical process to separate the charge (like a Van de Graaff generator).
- In a VDG generator, charge is mechanically transported. Also, since the electric field exerts a downward force on the positive charges moving up the belt. So, work must be done to lift the positive charges!

### Batteries and emf
- There is a charge escalator to bring positive charges to the negative terminal in a battery. This requires work (energy supplied by chemical reactions).
- **Work per charge = $epsilon$ = emf**. An ideal battery has potential difference of $\Delta V = \epsilon$. This is called terminal voltage.
- When **batteries are in series**, the total potential difference is the sum of their individual terminal voltages (i.e. individual potential differences). 

## Ch. 26.5: Capacitance and Capacitors
- Even if net charge is zero, separation of charges ($\pm Q$ can still create a potential difference.
- **The capacitance is: $C = \frac{Q}{\Delta V_C} = \frac{\epsilon_0A}{d}$; applies to PPC. Measured in farads (F = C/V).** $A$ is electrode surface area.
- Intuitively, it is the measure of the ability of an object to store energy in an electric field in response to a change in voltage.
- As a result, the charge on the capacitor plates is directly proportional to the potential difference between the plates: $Q = C\Delta V_C$.
- Capacitors are key for electric circuits because of their ability to store energy.
- Capacitance is a geometric quantity - depends only on the surface area and spacing of electrodes.

### Charging a Capacitor
- When capacitor is first connected to a battery: until capacitor fully charged, charge escalator moves charge from one plate to another and $\Delta V_C$.
- Fully charged capacitor: in equilibrium; capacitance only applied to fully-charged capacitors! Ions not moving here and current stops.

### Combinations of Capacitors
![](https://image.ibb.co/by20GJ/Screen_Shot_2018_05_14_at_11_12_38_AM.png)

- **Parallel capacitors:** joined top to top and bottom to bottom.
	- Total charge drawn from battery is $Q_1 + Q_2$.
	- Sum capacitance too!
- **Series capacitors:** joined end to end in a row
	- Total potential difference across both capacitors is $\Delta V_1 + \Delta V_2$.
	- $C_{eq} = (\frac{1}{C_1} + \frac{1}{C_2} + ...)^{-1}$

## Ch. 26.6: The Energy Stored in a Capacitor
- When the charge escalator does work to lift the charge $dq$ from the negative plate to the positive plate, the potential energy of the capacitor increases by $dU = dq\Delta V = \frac{qdq}{C}$. To find the total energy transferred from the battery to the capacitor, integrate and find that $U_C = \frac{Q^2}{2C} = \frac{C(\Delta V_C)^2}{2}$.
- Capacitor can be charged slowly but then release energy quickly (like defibrillator).
- Power is change in energy over time (measured in watts). If the same energy is released quickly, you get an enormous gain in power.
- When they ask for the "energy stored in a charged capacitor" they mean find $U_C$!
- **Energy density** = $energy stored/volume in which it is stored$; $u_E = \frac{U_C}{Ad}$ (measured in $J/m^3$).
- For a PPC, the volume is the product of the surface area of the plate and the distance between them.

## Ch. 26.7: Dielectrics
- **Dielectric:** an insulator in an electric field. (Note: pure distilled water is a good insulator.)
- Say you place insulating material between the PPC plates. The charge on the plates won't change, but the voltage will decrease and the capacitance will increase.
- Why? Because the neutral insulator will become polarized in the field. Excess negative charge will go on side towards positive plate. Excess positive charge will go on side towards negative plate. 
![](https://image.ibb.co/ngvbqd/Screen_Shot_2018_05_14_at_12_10_32_PM.png)
- The net electric field is $E = E_0 + E_{induced}$. Still points from positive to negative but with smaller magnitude.
- The dielectric constant is $\kappa = \frac{E_0}{E}$. Each material has one constant. If it's easily polarized, then it has a larger constant.
- In a vacuum, $\kappa = 1$.
- **Filling a capacitor with a dielectric increases the capacitance by a factor equal to the dielectric constant: $C = \kappa C_0$.**
- You want to increase capacitance, so most actual capacitors have some sort of liquid/solid dielectric.
- All materials have a maximum electric field they can sustain without breakdown (aka sparks). This maximum field is called the **dielectric strength**.
- The dielectric strength of air is $3 \times 10^6 V/m$.
- When a dielectric is introduced (especially one with a high constant), the polarized dielectric is pulled into the capacitor. You see a decrease in energy, which is the work that the capacitor's electric field did pulling in the dielectric.