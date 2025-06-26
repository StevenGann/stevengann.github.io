---
title: USB-Powered Night Light Circuit Analysis
description: Analysis of a DIY USB-powered night light circuit using a photoresistor and an NPN transistor
categories: [Electronics]
tags: [circuits, transistors, photoresistors, electronics, diy]
math: true
mermaid: true
---

## Introduction

A popular DIY project circulating online demonstrates how to construct a USB-powered night light using minimal components. While the videos provide excellent assembly instructions and soldering techniques, they lack detailed circuit analysis. I had a non-engineer friend ask me how the circuit works, so here's a detailed analysis.

## Circuit Overview

The night light circuit consists of a photoresistor-controlled LED array powered by a 5V USB supply. The core components include:

- **Power Supply**: 5V from USB port with common ground
- **LED Array**: Multiple LEDs connected in series/parallel configuration
- **Control Element**: NPN bipolar junction transistor (BJT)
- **Sensor**: Photoresistor for ambient light detection
- **Biasing Network**: Resistor network forming a voltage divider

The circuit operates on a simple principle: the photoresistor's resistance varies with ambient light levels, controlling the transistor's base current, which in turn regulates LED brightness.

## Transistor Fundamentals

The circuit employs an NPN bipolar junction transistor (BJT) as its primary control element. Understanding transistor operation is essential for circuit analysis.

An NPN transistor consists of three terminals:
- **Collector (C)**: High-voltage terminal where controlled current enters
- **Base (B)**: Control terminal that regulates collector current
- **Emitter (E)**: Low-voltage terminal where current exits

In the active region of operation, the transistor functions as a current-controlled current source. The collector-emitter current (I<sub>CE</sub>) is proportional to the base-emitter current (I<sub>BE</sub>), with the relationship defined by the current gain (β):

I<sub>CE</sub> = β × I<sub>BE</sub>

This amplification property makes transistors ideal for switching and amplification applications.

## Circuit Analysis

### Voltage Divider Configuration

The photoresistor and fixed resistor form a voltage divider network that controls the transistor's base voltage. The voltage at the divider's midpoint (V<sub>B</sub>) is given by:

V<sub>B</sub> = V<sub>CC</sub> × (R<sub>photo</sub> / (R<sub>photo</sub> + R<sub>fixed</sub>))

Where:
- V<sub>CC</sub> = 5V (USB supply voltage)
- R<sub>photo</sub> = Photoresistor resistance (varies with light)
- R<sub>fixed</sub> = Fixed resistor value

### Light-Dependent Operation

A photoresistor exhibits negative photoconductivity:
- **High light levels**: Low resistance (typically 1-10 kΩ)
- **Low light levels**: High resistance (typically 100kΩ-1MΩ)

This resistance variation directly affects the voltage divider output and, consequently, the base current.

### Current Control Mechanism

The base current (I<sub>B</sub>) is determined by:

I<sub>B</sub> = (V<sub>B</sub> - V<sub>BE</sub>) / R<sub>base</sub>

Where V<sub>BE</sub> ≈ 0.7V (typical base-emitter voltage drop).

As ambient light decreases:
1. Photoresistor resistance increases
2. Base voltage decreases
3. Base current decreases
4. Collector current decreases
5. LED brightness increases


## Design Considerations

- **Transistor**: Any general-purpose NPN transistor (2N2222, BC547, etc.)
- **Photoresistor**: CdS cell with appropriate resistance range, such as 100k Ohm
- **LEDs**: Standard 5mm LEDs with appropriate forward voltage
- **Resistors**: Values chosen to provide suitable biasing, most designs use a 10k Ohm resistor opposite the photoresistor, and a 1k Ohm resistor in series with the photoresistor

## Conclusion

The USB-powered night light circuit provides an excellent example of practical electronics design. By combining basic components with fundamental electronic principles, it creates a functional device that automatically responds to environmental conditions. Understanding the underlying circuit operation enables modification and extension for more sophisticated applications.