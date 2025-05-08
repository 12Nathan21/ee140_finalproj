# EE140 Final Project Overview

The GateKeepers: *Nathan Wong, Jiaming Zhao, Xingyi Lu*

## Division of Labor

### Power Supply

All modules related to the power supply, including <u>the BGR, the regulators and the op-amps inside them</u>, were implemented by Jiaming Zhao (JZ).

### MUX & Switches & Gates

All the switches (including the NMOS version, the PMOS version, and the “butterfly” CMOS transmission gates) were implemented by Xingyi Lu (XL). 

Nathan Wong (NW) picked the size for the devices which consist the inverters and the NAND gates.

XL also designed the the VBAT voltage divider.

### PGA

The folded cascode amplifier with a second stage op-amp was implemented and Miller-compensated by NW. The outer feedback loop and the switched-capacitors were designed and adjusted by XL.

### ADC

The comparator, based on the StrongArm Latch, was initially implemented by JZ. NW improved it by adding an SR latch at the output to eliminate potential race conditions.

XL contributes to the design of the cap DAC.

### Temp Sensing - BGT

JZ developed the initial mechanism of generating `VPTAT` based on the BGR. NW optimized the resistor values and added output buffers to improve performance.

## Modification on Testbench

- Top module: `EE_140_240A_Sp25_System`
- Submission module: `EE_140_240A_Sp25_Student_Submission_1`
- Testbench name: `EE_140`

> [!IMPORTANT]
>
> Dear Rahul or any other grading staff, please use ==Parameter Analysis== rather than running simulations with different values of the variables one by one. It is significantly more efficient, ~~and free of bugs~~.

1. Master CLK Delay

The master clock (with the net name `CLK`) frequency remains at 1 MHz, the same as in the original setup. However, we modified its start time — it now begins to rise at **1 µs** instead of 0.
This change was made because `VBAT` finishes ramping up at around 100 ns, and the rest of the circuit takes roughly another 600 ns to reach a steady state.

2. Simulation Time

Given the clock delay and the time required for the SAR FSM to complete all 8-bit conversions, we extended the total simulation time to **12.5 µs**.

3. Power Supply for the X_GSI Instance

We consulted Prof. Pister about the current spikes observed in the X_GSI module during simulation. He confirmed that the module should be powered by **VDDD** rather than VDDA.

As a result, we connected X_GSI to VDDD, which successfully prevented its current spikes from disrupting the AREG and the PGA.

## Experience & Feedback

The provided testbench appeared to have issues. For example, when we changed the value of `ext_in0`, the output waveforms did not update, and no waveform was plotted for the input signal itself.

Use Parameter Analysis solved this annoying problem.