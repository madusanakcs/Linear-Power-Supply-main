# 10V Linear Power Supply

## Abstract

The objective of this project was to design a 10V Linear power supply with a maximum current rating of 10A. This report presents the design of a voltage regulator from scratch to drive a high-power load (100W) from a 230V input voltage.

## Introduction

Power supplies are used to drive various loads under constant voltage/current conditions. To ensure the reliable operation of a power supply several factors need to be considered including load regulation, line regulation, efficiency, protection mechanism, heat dissipation, etc.

## Design Requirements

- 10V Linear power supply with a maximum current rating of 10A
- Should include circuit protection mechanisms.

## Methodology

### Rectification

The first stage of the DC power supply is rectification which involves converting the input sinusoidal AC voltage to an output voltage with a single polarity. There are many different techniques for implementing the rectification stage in a circuit, however, the most used and simplest method is using a Wheatstone bridge rectifier.

### Smoothing

The second stage is smoothing which reduces the ripple voltage in the output voltage of the rectifier output.

### Regulation

#### Voltage Regulation

The voltage regulation process is a crucial aspect of a linear power supply. To achieve a constant output voltage, a virtual short circuit is utilized in this design. Virtual short circuit refers to 
configuration in a differential amplifier such as op-amp where the inverting and non-invertinginput have almost same voltage. Initially 15v voltage is regulated and 10V is supplied to one of the op-amp terminals,ensuring a constant output voltage of 10V. The op amp is supplied with 15V and 0V to ensure that positive voltage is fed back to the Darlington. In order to further regulate the voltage to the desired level of 12V, a Zener diode is implemented.

![schematic](https://github.com/Nusrath-Amana/Linear-Power-Supply/blob/main/PCB%20and%20schematics/images/regu_sch.png)

#### Current regulation
TIP142 Darlington pair is utilized to control the current. The op-amp provides feedback to the Darlington pair and by using that control the current. Here, the Darlington pair work as a current controlling gate.

### Current Limiting

This power supply is required to limit the maximum current up to 10A. When the current through a circuit exceeds its rated current, it may cause overheating, and component failure. Thus, implementing an overcurrent protection circuit, prevent damage to the device and ensures proper functionality.

The design uses two SPDT relays. An NPN transistor (Q1) is added in series with the coil, along with a 1k resistor (R6) between the supply voltage and the base of the transistor. A flyback diode is added to prevent the occurrence of huge voltage spikes when the relay coil is de-energized. A green LED is used to indicate the absence of over current problem.

To deactivate the coil when an over-current issue occurs, an NPN transistor (Q2) is added to the base of the first transistor. If an error signal is applied to the base of the second transistor (Q2), the base current of the first transistor (Q1) will flow through the second transistor. This causes the coil to deactivate, the LED to turn off, and the contacts to open.

To detect current exceeding 10A a low-value power resistor (Res2) is added between the supply voltage and the first relay contact. This creates a voltage drop proportional to the flowing current, which is amplified using an operational amplifier (op-amp U4A) in differential amplification configuration.

The amplified signal is connected to the non-inverting input of the second op-amp (U4B), whose inverting input is directly connected to the potentiometer, which acts as a variable reference voltage. Since the op-amp functions as a comparator, its output is pulled high when the current sense voltage is higher than the reference voltage. This triggers outputs that are connected to the base of the second transistor (Q2) through a resistor (R5), thus turning off the relay when the current exceeds 10A.

However, once the relay is no longer activated, the flowing current decreases and turns off the output of the comparator, allowing the relay to be activated once again. Since over-current will flow once again when the relay is activated, the comparator triggers once again, and the cycle repeats repeatedly.

To address this, a reset switch (S1) (normally closed push button) with a resistor (R7) is connected between the normally closed contact of the relay K4 and the base of Q2. When current exceeds 10A, the relay turns off, but since normally closed contact is closed, base of Q2 is still pulled to supply voltage even though comparator output is low. As a result, relay remains off until reset button is pushed which interrupts base current of Q2 allowing relay to be activated once again.

![schematic](https://github.com/Nusrath-Amana/Linear-Power-Supply/blob/main/PCB%20and%20schematics/images/short%20cct_sch.png)

### Protection

#### Short Circuit Protection

The current limiting circuit discussed in section 2.4 is used as short circuit protection. When a short circuit occurs, the current limiting circuit will be activated.

#### Overcurrent Protection

A 12A 250V Fast Blow Glass Fuse (AC fuse) is utilized to implement overcurrent protection. The maximum current allows through the fuse is 12A. 

#### Thermal Protection

Thermal protection is an important consideration in linear power supplies to protect against overheating and potential damage to the circuit. In this design, Heatsinks, Fans and ventilation, and proper PCB design are utilized to implement thermal protection.

## PCB designs
### PCB design of regulation circuit
![pcb1](https://github.com/Nusrath-Amana/Linear-Power-Supply/blob/main/PCB%20and%20schematics/images/regu_pcb.png)

### PCB design of current limiting circuit
![pcb1](https://github.com/Nusrath-Amana/Linear-Power-Supply/blob/main/PCB%20and%20schematics/images/short%20cct_pcb.png)

## Conclusion

This project was finalized with two PCB and a 3D-printed enclosure to house the PCB. The power supply was successfully able to drive a high-power load (100W) from a 230V input voltage.
