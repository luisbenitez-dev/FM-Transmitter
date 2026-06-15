# Discrete Component FM Transmitter
## Overview
This repository contains the LTspice simulation and theoretical analysis of a two-stage FM transmitter. The circuit consists of an audio preamplifier stage coupled to a radio frequency (RF) Colpitts oscillator, functioning as a voltage-controlled oscillator (VCO) to achieve frequency modulation.

## Circuit Architecture
![FM Transmitter Schematic](fm-transmitter-schematic.png)

The design is split into two functional blocks:
1. **Audio Preamplifier (Q2 - 2N3904):** Amplifies the baseband input signal (simulated as a 1 kHz sine wave).
2. **RF Oscillator & Modulator (Q1 - 2N2222):** A Colpitts oscillator generating the high-frequency carrier. The amplified audio signal modulates the base bias of Q1. This dynamic biasing alters the internal junction capacitances of the transistor (Cbc and Cbe), shifting the resonant frequency of the LC tank to produce FM.

## Simulation & Analysis
### 1. Fast Fourier Transform (FFT) - Carrier Frequency
![FFT Analysis](fm-transmitter-fft.png)
The ideal resonant frequency of the LC tank (L1 = 0.1uH, C2 = 33pF, C3 = 47pF) without parasitic elements is calculated as follows:

Ceq = (33 x 47)/(33 + 47) ~ 19.38pF

fc = 1/(2*pi*sqrt(L*Ceq)) ~ 114.3MHz

The FFT confirms a carrier frequency peak at approximately 100 MHz. The downward shift from the ideal 114.3 MHz is caused by the parallel addition of the 2N2222's internal junction capacitances to the tank circuit.

### 2. Transient Analysis & Incidental AM
![Transient Analysis](fm-transmitter-transient.png)
The time-domain simulation reveals the modulated high-frequency carrier. Notably, the signal exhibits an amplitude envelope varying at the 1 kHz baseband frequency. 

This **incidental Amplitude Modulation (AM)** is a known artifact of simple discrete FM transmitters. Because the audio signal alters the DC operating point (bias) of the oscillator to achieve frequency deviation, it simultaneously changes the transistor's transconductance (gm) and loop gain, causing the output amplitude to fluctuate alongside the frequency.

## Conclusions
1. **VCO Operation via Parasitic Modulation:** Frequency modulation was successfully achieved without a dedicated varactor diode by exploiting the voltage-dependent nature of the 2N2222's internal junction capacitances (Cbe and Cbc). This demonstrates how parasitic parameters can be leveraged for functional circuit behavior.
2. **Design Trade-offs in Single-Stage RF:** Integrating the oscillator and modulator into a single transistor stage (Q1) severely compromises signal purity. The presence of significant incidental AM proves that independent oscillator and modulation stages are necessary for high-fidelity telecommunication applications.
3. **Simulation Accuracy vs. Ideal Theory:** The 14.3 MHz discrepancy between the ideal LC tank equation (114.3MHz) and the SPICE simulation (100MHz) highlights the critical importance of utilizing accurate active component models in RF design, as sub-nanofarad parasitic parameters dominate circuit behavior at VHF bands.

## Full Engineering Report
For a comprehensive analysis including complete mathematical breakdowns, physical measurements, and component justifications, please refer to the formal documentation:

[📄 Download/View Technical Report](fm-transmitter-technical-report.pdf)