# Solar Charge Protection Module (v1.0 Baseline)

## Project Overview
A robust, hardware-level protection baseline for solar charging systems. This module is designed to prevent reverse current leakage from batteries to solar panels during low-light conditions and protect the battery from overvoltage events using a Zener-clamped dissipation architecture. 

This repository serves as the foundational hardware layer (v1.0) before transitioning to active microcontroller-based power management.

## Hardware Architecture & Component Selection
Instead of blindly relying on generic components, each part was selected with specific power-efficiency and safety metrics in mind:
* **Reverse Current Protection (D1 - 1N5819 Schottky Diode):** Selected over standard silicon diodes (e.g., 1N4007) to minimize forward voltage drop (approx. 0.2V - 0.3V vs. 0.7V). This significantly reduces thermal dissipation (W = V * I) and maximizes power transfer efficiency from the panel to the battery.
* **Overvoltage Clamping (D2 - 15V Zener Diode):** Configured as a shunt regulator to ground. If the solar panel output exceeds the safe 15V threshold, the Zener diode enters breakdown mode, safely routing excess dangerous currents to GND and locking the system voltage at 15V to prevent battery swelling or explosion.
* **Current Limiting (R1 - 10R Resistor):** Prevents catastrophic current surges through the Zener diode during extreme overvoltage events.

## Simulation & Validation (LTspice)
Prior to physical PCB layout, the electrical limits of the circuit were strictly validated using LTspice to ensure thermal stability:
* **Clamping Verification:** Simulated with an 18V ideal pulse source and a realistic internal series resistance. The node voltage successfully flatlined at exactly 15V, proving the Zener diode's protection capability.
* **Dynamic Response:** Verified the zero-current state of the Zener diode when system voltage is within safe limits (<15V).

<img width="1918" height="821" alt="1" src="https://github.com/user-attachments/assets/1e014473-4a85-4711-80ed-7dbfced5291c" />

* **Red Trace `V(n001)`:** Solar Panel Input Voltage (surging to ~18V).
* **Blue Trace `V(n002)`:** Node/Battery Voltage (safely clamped at the 15V limit).
* **Green Trace `-I(D2)`:** Zener Diode Shunt Current (activating only when input > 15V, limited to safe levels by R1).

## PCB Layout & Physical Design (KiCad)
The conceptual schematic was translated into a manufacturing-ready physical board using KiCad:
* **High-Current Routing:** All power traces on the bottom copper layer (`B.Cu`) are routed at a robust **1.0 mm width** to handle sustained solar currents without thermal degradation.
* **Mechanical Stability:** Utilized Through-Hole Technology (THT) and robust Phoenix screw terminals for mechanical reliability in physical field-testing environments.

<img width="922" height="620" alt="2" src="https://github.com/user-attachments/assets/2be8e36a-9201-477d-8db9-14a9569b6f67" />


## Future Roadmap
* **v2.0 (Active MPPT):** Upgrading this passive protection baseline by integrating an STM32 microcontroller and a Buck Converter topology to achieve >90% efficiency using Maximum Power Point Tracking (MPPT) algorithms, eliminating Zener thermal losses.
