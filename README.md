Single-Supply Active Audio Mixer & Preamp
Status: Functional Prototype (Optimization Phase)
1. Project Overview
This project is a custom analog audio mixer designed to integrate high-impedance instruments (Piezo Guitar Pickups) with line-level consumer audio (Smartphones) into a portable speaker system. Unlike standard dual-supply operational amplifier circuits, this design operates on a Single-Supply (12V) topology derived from a 3.7V Li-ion source.
2. System Architecture (Experimental Setup)
The circuit consists of three distinct stages:
A. Power Management Stage
• Source: 3.7V Li-ion Cell (18650).
• Regulation: MT3608/XL6009 DC-DC Boost Converter stepping 3.7V up to 12V.
• Noise Filtering: A custom RC Pi-Filter (4×47Ω resistors + 1000μF capacitor) was implemented to reject high-frequency switching noise from the boost converter.
B. Preamplification & Summing Stage (The "Fishman" Killer)
• Core IC: RC4558 Dual Op-Amp.
• Topology: Inverting Summing Amplifier with AC Coupling.
• Biasing: A Virtual Ground (V 
ref
​
 ) was established at V 
CC
​
 /2 (approx. 6V) using a resistive voltage divider to allow full AC signal swing on a single rail, following Texas Instruments Application Report SLOA058.
• Impedance Matching:
    ◦ Guitar Input: High-gain path (R 
in
​
 ≈7.3kΩ).
    ◦ Line Input: Low-gain path (R 
in
​
 ≈47kΩ) (Optimized from 122kΩ to restore signal presence).
C. Power Amplification Stage
• Core IC: TDA2822M (configured in Stereo/Bridge mode).
• Output: Direct drive to 3W speaker load.
3. Implementation Details
Component
Value
Function
Op-Amp
RC4558
Summing Preamp & Active Tone Stack
Power Amp
TDA2822M
Output Stage Driver
V 
ref
​
  Resistors
10kΩ / 10kΩ
Creates 6V Virtual Ground (Voltage Divider)
Filter Cap
1000μF
Bulk Decoupling for Power Rail
 Figure 1: Full system test bench showing signal injection from mobile device and power regulation.
 Figure 2: Close-up of the DC-DC Booster (Blue) and the TDA2822M Power Stage (Green).
4. Engineering Challenges & Solutions
Issue 1: Virtual Ground Imbalance (The "Silence")
• Symptom: Output signal was heavily clipped/silent; V 
ref
​
  measured at 2.5V instead of 6V.
• Root Cause: Resistive imbalance in the voltage divider network and a short-circuit on Op-Amp Pin 5.
• Solution: Balanced the divider resistors to identical values and cleared the solder bridge, restoring V 
ref
​
  to 5.84V and enabling full signal swing.
Issue 2: Ground Loop Hum
• Symptom: 50Hz hum that increased when touching the circuit.
• Root Cause: Floating ground reference acting as an antenna.
• Solution: Unified "Star Ground" topology and chassis bonding.
5. Future Optimization (Known Issues)
• Noise Floor: A residual hiss (Johnson Noise) persists due to unshielded point-to-point wiring. Next revision will utilize shielded coaxial cable for input signal paths.
• Headroom: Loud transient strums cause minor clipping ("cracking"). Gain feedback resistors (R 
f
​
 ) will be adjusted to increase headroom.

--------------------------------------------------------------------------------
Phase 4: Why This Framework Works
1. It uses "Smart" Words: You aren't "fixing a buzz"; you are "resolving a Ground Loop." You aren't "making it louder"; you are "optimizing Impedance Matching."
2. It Cites Standards: Referencing TI SLOA058 shows you read the datasheet and didn't just guess.
3. It Admits Faults: Section 5 (Known Issues) is the most professional part. It tells a recruiter, "I know exactly why it isn't perfect yet," which is infinitely better than claiming it is perfect when it isn't.
Action Item: Go create this Repo right now. Once it's live, you can drop the link into your LinkedIn profile.
