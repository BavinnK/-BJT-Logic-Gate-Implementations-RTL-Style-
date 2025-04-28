# BJT Logic Gate Implementations (RTL Style)

This repository contains simple circuit diagrams demonstrating the implementation of basic digital logic gates (NOT, NAND, NOR, OR) using Bipolar Junction Transistors (BJTs) in a Resistor-Transistor Logic (RTL) style configuration.

## Introduction

Digital electronics rely on logic gates to perform operations. While modern integrated circuits (ICs) contain millions or billions of transistors packed into a small chip, understanding the fundamental building blocks is crucial. This project shows how basic logic functions can be realized using discrete components like NPN BJT transistors (BC547 / 2N2222) and resistors.

These circuits operate based on the principle of using the BJT as an electronic switch.

*   **Logic LOW (0):** Input voltage is near 0V. The transistor is in **cutoff** (OFF).
*   **Logic HIGH (1):** Input voltage is sufficiently high (e.g., close to Vcc, or at least > 0.7V to turn the base-emitter junction on). The transistor is driven into **saturation** (ON).

## Gates Implemented

*   [NOT (Inverter)](#not-inverter)
*   [NAND](#nand-gate)
*   [NOR](#nor-gate)
*   [OR](#or-gate)

## Components Used

*   **Transistors:** NPN BJT (BC547 or 2N2222 are suitable)
*   **Resistors:** 1 kΩ (as shown in diagrams)
*   **Power Supply:** Vcc (e.g., +5V is common for digital logic experiments)
*   **Ground:** 0V reference

---

## Circuit Diagrams & Explanation

### NOT (Inverter)

The inverter outputs the opposite logic level of the input.

**Circuit Diagram:**
![inverterD](https://github.com/user-attachments/assets/ace7212e-03be-48b3-8577-b16dc25f5add)

![inverter](https://github.com/user-attachments/assets/74ac7dfe-b7c4-487d-8fd8-8fc4c9ab9f63)


**Operation:**
*   **Input HIGH:** `Vin` is HIGH. Base current flows through the 1kΩ resistor into the base of the transistor. The transistor turns ON (saturates). The collector is pulled down close to ground potential. `Vout` is LOW.
*   **Input LOW:** `Vin` is LOW. No significant base current flows. The transistor is OFF (cutoff). No current flows through the collector resistor to ground via the transistor. `Vout` is pulled HIGH towards Vcc through the 1kΩ collector resistor.

---

### NAND Gate

The NAND gate outputs LOW only if *all* inputs are HIGH. Otherwise, the output is HIGH.

**Circuit Diagram:**
![nandD](https://github.com/user-attachments/assets/1dc343a9-e7b2-4462-9fed-7ef60758c812)

![nand](https://github.com/user-attachments/assets/f0e91a3d-3832-4b15-9a61-1805e4f416c2)


**Operation:**
*   The two transistors are connected in series. For current to flow from Vcc through the collector resistor to ground, *both* transistors must be turned ON.
*   **Any Input LOW:** If either input A or input B (or both) is LOW, the corresponding transistor(s) will be OFF. The path to ground is broken. `out` is pulled HIGH towards Vcc through the collector resistor.
*   **All Inputs HIGH:** If both input A and input B are HIGH, both transistors turn ON. A path to ground is created through the series transistors. `out` is pulled LOW.

---

### NOR Gate

The NOR gate outputs LOW if *any* input is HIGH. The output is HIGH only if *all* inputs are LOW.

**Circuit Diagram:**
![nord](https://github.com/user-attachments/assets/53a214bc-dddc-4688-9f55-0795b5dd8495)

![nor](https://github.com/user-attachments/assets/11c51939-0b2b-4497-bfa8-ab8e4c6d504c)


**Operation:**
*   The two transistors are connected in parallel, sharing a common collector resistor. If *either* transistor turns ON, it will pull the output node LOW.
*   **Any Input HIGH:** If input A or input B (or both) is HIGH, the corresponding transistor(s) will turn ON. A path to ground is created through the ON transistor(s). `out` is pulled LOW.
*   **All Inputs LOW:** If both input A and input B are LOW, both transistors remain OFF. No path to ground is created through the transistors. `out` is pulled HIGH towards Vcc through the collector resistor.

---

### OR Gate

The OR gate outputs HIGH if *any* input is HIGH. The output is LOW only if *all* inputs are LOW.
*(Note: This specific implementation uses a NOR gate followed by an Inverter to achieve the OR function: OR = NOT(NOR)).*

**Circuit Diagram:**
![orD](https://github.com/user-attachments/assets/4b31ebe0-3e8f-40ad-9211-223b4b4a7f68)


![or](https://github.com/user-attachments/assets/992435f1-2faa-4366-baae-454fe7a0ebb2)


**Operation:**
*   The first stage consists of two transistors whose collectors are tied together (forming a NOR-like structure, though without its own pull-up resistor shown explicitly driving the next stage base).
*   **Any Input HIGH:** If input A or input B (or both) is HIGH, the corresponding first-stage transistor(s) turn ON. This pulls the input node of the *second stage* (the base of the final transistor) LOW.
*   With the base of the final transistor LOW, it turns OFF.
*   Since the final transistor is OFF, the `out` node is pulled HIGH towards Vcc through its 1kΩ collector resistor.
*   **All Inputs LOW:** If both input A and input B are LOW, both first-stage transistors are OFF. The input node to the second stage is pulled HIGH (implicitly, through the collector resistor of the *first* stage, which is connected to Vcc - although not drawn optimally, this is the intent).
*   With the base of the final transistor HIGH, it turns ON.
*   Since the final transistor is ON, the `out` node is pulled LOW towards ground.

*Summary:* This circuit outputs HIGH if A OR B is HIGH, and LOW only if A AND B are LOW, correctly implementing the OR function.

---

## Usage Notes

*   **Logic Levels:** For these circuits with Vcc = 5V:
    *   Logic HIGH (1) is typically close to Vcc (e.g., > 3.5V, ideally ~5V when outputting HIGH).
    *   Logic LOW (0) is typically close to Ground (e.g., < 0.7V, ideally ~0.2V when outputting LOW).
*   **Input Voltage:** Ensure `Vin` HIGH is sufficient to saturate the transistor (generally > 0.7V) and `Vin` LOW is close to 0V.
*   **Limitations:** RTL logic is simple but has limitations compared to modern logic families (like TTL or CMOS), including:
    *   Lower speed.
    *   Lower fan-out (ability to drive multiple inputs).
    *   Potential for voltage level degradation when cascading gates.
    *   Higher power consumption when the output is LOW (current flows through collector resistor and saturated transistor).

## Future Work

*   [ ] Include simulation results (e.g., using SPICE or an online simulator).
*   [ ] Explore implementations of other gates (XOR, XNOR).
*   [ ] Analyze performance characteristics (propagation delay, power consumption).

