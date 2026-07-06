# Project Overview

## Purpose

This project is an educational three-lead ECG acquisition prototype designed to acquire and visualize cardiac electrical activity using an analog front-end, filtering stages, and digital acquisition.

The goal is to study the design of biomedical signal conditioning circuits, especially for low-amplitude biopotential signals such as ECG.

This prototype is not a certified medical device and must not be used for diagnosis, treatment, monitoring, or clinical decision-making.

## Main Objectives

- Build a basic ECG analog front-end.
- Measure low-amplitude biopotential signals using surface electrodes.
- Reduce baseline drift and high-frequency noise using analog filters.
- Acquire the conditioned signal using a microcontroller or external ADC.
- Visualize the ECG waveform using Python or other software.
- Document the design, tests, limitations, and future improvements.

## System Architecture

The system follows this general signal path:

```mermaid
flowchart LR
    A[Surface electrodes] --> B[Instrumentation amplifier]
    B --> C[High-pass filter]
    C --> D[Gain stage]
    D --> E[Low-pass filter]
    E --> F[ADC / Microcontroller]
    F --> G[PC visualization software]
```


## Electrode Configuration

The prototype uses a basic three-electrode configuration:

- RA: Right arm electrode
- LA: Left arm electrode
- RL: Right leg reference / bias electrode

The ECG signal is obtained from the potential difference between the measurement electrodes. The right leg electrode is used as a reference or bias electrode, depending on the circuit configuration.

## Current Prototype Status

The first prototype has been built and tested. It can show ECG-like waveforms, but the signal still contains noise and requires improvement.

## Current known issues include:

- 60 Hz mains interference
- Baseline drift
- Motion artifacts
- Electrode contact noise
- Breadboard parasitic effects
- Sensitivity to cable movement
- Need for better shielding and grounding
- Need for improved digital filtering

## The current prototype uses the following main elements:

- AD620 instrumentation amplifier
- Operational amplifiers for filtering and gain stages
- Passive RC components (1% resistances and polypropylene capacitor)
- ECG surface electrodes
- Shielded cable
- ESP32
- ADS1115 External ADC 
- Oscilloscope for signal testing
- Function generator for filter testing
- Battery or isolated power supply for safer testing
- Software Summary

## The digital acquisition and visualization stage may include:

- Serial communication with the microcontroller
- Real-time ECG plotting
- CSV data recording
- Digital filtering
- Heart rate estimation
- Signal analysis using Python

## Safety Considerations

This project must only be tested using isolated battery power when electrodes are attached to a person.
The circuit must not be connected directly to mains-powered equipment while attached to a person unless proper medical-grade isolation is implemented.
Oscilloscopes, bench power supplies, PCs, and USB-connected devices may introduce grounding and isolation risks. Any human-connected test must be treated carefully.

## Limitations

This prototype is educational and experimental. It is not suitable for medical use.

Main limitations:

- It has not been validated with a calibrated ECG simulator.
- It does not comply with medical safety standards.
- It does not include medical-grade isolation.
- It is sensitive to noise and movement.
- The signal quality depends strongly on electrode placement and cable shielding.
- The current design still requires more testing and optimization.

##Future Work

Planned future improvements:

- Design a proper PCB.
- Add a driven-right-leg circuit.
- Improve shielding and cable routing.
- Improve ADC resolution and sampling stability.
-Validate the circuit using an ECG simulator.
-Compare the acquired waveform against known ECG references.
-Improve documentation with photos, oscilloscope captures, and test data.
