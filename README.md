# Boost Converter (XL6009)

A DC-DC step-up (boost) converter designed in KiCad, built around the **XL6009** switching regulator IC — takes a lower input DC voltage and boosts it to a higher, adjustable regulated output voltage.

## Why This Exists

Many projects have a power source that's lower than what a circuit actually needs — e.g. boosting a single Li-ion cell (~3.7V) up to 12V, or a 5V USB source up to 9V/12V for other electronics. A linear regulator can't do this (it can only step voltage *down*), so a switching boost converter is required. This board uses the XL6009 to do that conversion efficiently, with an adjustable output via an onboard potentiometer.

## Circuit Overview

- **Input**: J1/J2 2-pin connector bringing in input DC voltage and GND
- **Input filtering**: C1 (47µF) for bulk smoothing, C2 (1µF) for high-frequency noise filtering
- **Core regulator**: XL6009 (VR1) boost converter IC — handles the actual switching/voltage step-up
- **Inductor**: L1 (33µH, 4A) — energy storage element central to the boost topology
- **Rectifier diode**: D1 (SS34 Schottky) — low forward-drop, fast-switching diode for the output rectification path
- **Feedback/voltage adjustment**: RV1 (potentiometer) + R1 (1kΩ) form a feedback divider into the XL6009's FB pin, letting you tune the output voltage
- **Output filtering**: C3 (1µF) and C4 (220µF) smooth the regulated output
- **Output**: J3/J4 2-pin connector delivering the boosted, regulated DC voltage to the load

## Signal Flow (Concept)

**Energy conversion path:**
1. Input DC voltage enters through J1/J2, filtered by C1/C2.
2. The XL6009's internal switch rapidly turns ON/OFF at the SW pin, connected to inductor L1.
3. **Switch ON:** Current flows from VIN through L1, storing energy in its magnetic field. D1 blocks reverse current into the output during this phase.
4. **Switch OFF:** L1's stored energy releases, and its voltage adds on top of VIN, pushing current through D1 into the output capacitors at a higher voltage than the input.
5. This switching repeats at high frequency (hundreds of kHz), continuously pumping energy to the output.
6. C3/C4 smooth the resulting output into clean, stable DC.
7. Regulated output is delivered to the load via J3/J4.

**Feedback / voltage regulation:**
- The FB pin on the XL6009 monitors a scaled-down sample of the output voltage via the RV1/R1 resistor divider.
- If output voltage drifts from the set point, the XL6009 adjusts its switching duty cycle (how long the switch stays ON per cycle) to correct it.
- Adjusting RV1 changes the divider ratio, which shifts what output voltage the chip regulates to — this is how the output voltage becomes user-adjustable.

**Enable / control:**
- The EN pin is tied active, so the regulator runs continuously once powered. (Can be repurposed for external ON/OFF control if needed.)

**Ground referencing:**
- GND is shared across input, IC, and output sections — required for both the switching regulation loop and correct feedback sensing to function properly.

## Schematic
![Schematic](https://github.com/Noraiz963/Boost-Converter-/blob/3b7b14ee1ab4356e1747960dd5e4eab89ccdad8f/SCH%20(2).png)

## PCB Layout
![PCB Layout](https://github.com/Noraiz963/Boost-Converter-/blob/3b7b14ee1ab4356e1747960dd5e4eab89ccdad8f/PCB(boost).png)

## 3D Render
![3D Render](https://github.com/Noraiz963/Boost-Converter-/blob/3b7b14ee1ab4356e1747960dd5e4eab89ccdad8f/3d(Boost-Converter).png)

## Bill of Materials

| Ref | Component | Value |
|-----|-----------|-------|
| J1, J2 | Input Connector (2x 1-pin) | — |
| J3, J4 | Output Connector (2x 1-pin) | — |
| VR1 | Boost Converter IC | XL6009 |
| L1 | Inductor | 33µH, 4A |
| D1 | Schottky Diode | SS34 |
| RV1 | Potentiometer (feedback adjust) | — |
| R1 | Resistor | 1kΩ |
| C1 | Capacitor (input bulk) | 47µF |
| C2 | Capacitor (input HF filter) | 1µF |
| C3 | Capacitor (output HF filter) | 1µF |
| C4 | Capacitor (output bulk) | 220µF |

## Specifications

| Parameter | Value |
|---|---|
| Topology | Boost (step-up) |
| Regulator IC | XL6009 |
| Inductor | 33µH / 4A |
| Output adjustment | Via onboard potentiometer (RV1) |
| Rectifier | SS34 Schottky diode |

> ⚠️ Always verify output voltage with a multimeter before connecting a load — adjust RV1 to your target voltage first, and stay within the XL6009's rated input/output limits and current capacity of L1/D1.

## Tools Used
- KiCad 10.0.4 (schematic capture, PCB layout, 3D visualization)

## Status
Completed schematic design.

## Notes
Built as a learning project to understand switching boost converter topology, inductor-based energy storage, and feedback-based voltage regulation — a practical, reusable module for future projects needing a higher voltage rail than the available supply.
