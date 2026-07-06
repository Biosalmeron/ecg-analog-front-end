# Three-Lead ECG Acquisition System

This project is an educational three-lead ECG acquisition prototype designed to measure and visualize cardiac electrical activity using an analog front-end, filtering stages, and digital acquisition.

The system was developed as a biomedical engineering project focused on analog signal conditioning, biopotential measurement, noise reduction, and basic physiological signal visualization.

This device is not a certified medical device and must not be used for diagnosis, treatment, patient monitoring, or any clinical decision-making.

## Project Goals

- Design a basic ECG analog front-end for low-amplitude biopotential signals.
- Implement signal conditioning stages for amplification and filtering.
- Acquire the ECG signal using a microcontroller or ADC.
- Visualize the signal using Python, MATLAB, or serial plotting tools.
- Document the design process, problems, tests, and limitations.

## System Overview

The prototype uses surface electrodes connected in a three-lead configuration:

- RA: Right arm electrode
- LA: Left arm electrode
- RL: Right leg reference / bias electrode

The analog front-end includes:

1. Instrumentation amplifier stage
2. High-pass filter for baseline drift reduction
3. Gain stage
4. Low-pass filter for high-frequency noise reduction
5. Optional 60 Hz notch filtering
6. ADC acquisition and digital visualization

## Hardware Used

- AD620 instrumentation amplifier
- TL074 Operational amplifiers for filtering and buffering
- Passive RC filter components (Using 1% resistors and poplyproplylene capacitors)
- Surface ECG electrodes
- Shielded cable
- ESP32 and external ADC (ADS1115)
- Oscilloscope and function generator for testing (Models used: Rigol DHO804 and AWG Xr2206)

## Current Status

The prototype can acquire ECG-like waveforms, but the signal still contains noise and requires further improvement in shielding, filtering, grounding, electrode placement, and digital processing, the project has been tested usign a breadbord,
currently I am working in a pcb, the schematics and gerbers for using it with dual power suplies, or simple power suplies are in this respective folder. This project has not real galvanic isolation or any other proctive meassures, for V2 i am planning 
to add isoleted power suplies using B05S05 or an isolation amplifier like ISO124P.

## Safety Notice

This project must be powered only from isolated battery supplies during human testing. It must never be connected directly to mains-powered equipment while electrodes are attached to a person unless proper medical-grade isolation is implemented.

## Repository Structure

- `docs/`: technical documentation and design explanation
- `hardware/`: schematics, simulations, PCB files, and bill of materials
- `firmware/`: microcontroller code
- `software/`: Python visualization tools
- `data/`: test signals and measurements
- `media/`: photos, oscilloscope captures, and diagrams
- `references/`: useful papers, books, and standards

## Future Work

- Improve 60 Hz noise rejection
- Design a proper PCB
- Add driven-right-leg circuit
- Improve ADC resolution and sampling stability
- Add digital filters
- Estimate heart rate automatically
- Validate the circuit using a calibrated ECG simulator
- Add galvanic isolation
