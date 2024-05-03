# ESC-analysis
Hey there! In this repository, I'm sharing a study I did on a cheap Electronic Speed Controller (ESC).</br>
I was curious about how it worked, so I took it apart to see what makes it tick.</br>
In this repo, you'll find everything I discovered during my little reverse-engineering adventure.</br>
This is what I found and how I did it:

<h2>Hardware</h2>

First things first, I opened up an actual ESC to see what's inside. Here's what I found:

<h3>Bottom View</h3>

![Bottom View of the PCB](/Images/BottomView.jpeg)

<h3>Top View</h3>

![Top View of the PCB](/Images/TopView.jpeg)

Let's take a look at the physical components and explore where these marking codes lead us:

- **ATmega8L**: This is a microcontroller. You can read the [datasheet](Datasheets/MICROCONTROLLER_ATmega8L.pdf) for more details.
- **T4**: This is a diode, and its marking code refers to the 1N4148W. You can read the [datasheet](Datasheets/DIODE_1N4148W.pdf) for more details.
- **DTC**: This is a transistor with some resistors built in. You can read the [datasheet](Datasheets/TRANSISTOR_DTC143Z.pdf) for more details.
- **78M05**: This is a 5V voltage regulator (L78M05CDT). You can read the [datasheet](Datasheets/REGULATOR_L78M05CDT.pdf) for more details.
- **LM78L05ACM**: This is also a 5V voltage regulator but with different packaging and maximum current capacity. You can read the [datasheet](Datasheets/REGULATOR_LM78L05ACM.pdf) for more details.
- **PD0603BDL**: This is a MOSFET. You can read the [datasheet](Datasheets/MOSFET_PD0603BDL.pdf) for more details.

<h3>Schematics</h3>
With that information, I could start measuring the traces with a multimeter, checking them one by one, and then transfer that information to the schematics.</br>
However, I'll show you the schematic in a broken-down and separated format to simplify the explanation. 


</br>// But if you want to see the complete schematics [click here](ESC_Analysis_Schematic.PDF)

<h4>Power Supply Stage</h4>

![Power Supply Schematics](/Schematics/PowerSupply.png)

In this section of the circuit, we have all the power rails. First, there's the signal connector, which is connected to the 5V rail. This 5V is provided by two [L78M05CDT](Datasheets/REGULATOR_L78M05CDT.pdf) voltage regulators. 
Next, we see the power source connection in parallel with a capacitor, followed by an [LM78L05ACM](Datasheets/REGULATOR_LM78L05ACM.pdf) voltage regulator, which supplies 5V to the microcontroller. 
This regulator helps prevent surges, voltage drops, or other interferences that could affect the microcontroller's operation.


<h4>Microcontroller</h4>

![Microcontroller Schematics](/Schematics/Microcontroller.png)

The [ATmega8L](Datasheets/MICROCONTROLLER_ATmega8L.pdf) is responsible for all the magic here. It coordinates the switching of the MOSFETs, controls the speed, monitors the power source for any disturbances, and more.

<h4>MOSFETs Switching</h4>

![High-Side Switching Schematics](/Schematics/HS_Switching.png)
![Low-Side Switching Schematics](/Schematics/LS_Switching.png)

In these two sections, we have a circuit that, among other things, helps the ATmega to properly switch the MOSFETs.

<h4>Three Phase H-Bridge</h4>

![Three-Phase H-Bridge Schematics](/Schematics/ThreePhaseBridge.png)

These MOSFETs are responsible for properly switching the voltage to the motor, using a simple three-phase system.
</br>
</br>Here's a brief explanation of how the H-Bridge works:
</br>In a three-phase H-Bridge, there are typically six MOSFETs arranged in three pairs. Each pair controls one phase of the motor, allowing for precise control of motor speed and direction, by turning the MOSFETs on and off in a specific sequence. 
This type of setup is commonly used in brushless DC (BLDC) motors, which require accurate control of the phase timing for smooth operation.

<h4>Sense Circuits</h4>

![Phase Senses Schematics](/Schematics/PhaseSense.png)
![Vdd Sense Schematics](/Schematics/VddSense.png)

Last but not least, we have a simple resistor-based circuit that provides the microcontroller with references on how things are going and whether there are any issues with the motor, such as overcurrent or malfunction.

<h3>Building the PCB layout</h3>

With the schematics and the real board in hand, I was able to start building the 3D version of the PCB. However, without desoldering the components, I couldn't be certain about the exact locations of some things, especially certain traces in the inner layers. So, I made some educated guesses, and the results are shown below:

#### 3D Board

##### Top View

![3D Top View](/Images/3D_Top_View.png)

##### Bottom View

![3D Bottom View](/Images/3d_Bottom_View.png)

### Copper Layers

#### Top Layer

![Top Copper Layer](/Images/Top_Copper.png)

#### Inner Layer

![Inner Copper Layer](/Images/Inner_Copper.png)

#### Bottom Layer

![Bottom Copper Layer](/Images/Bottom_Copper.png)

COMING SOON...
