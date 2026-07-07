# Analog Front-End

## Overview

The analog front-end is responsible for conditioning the ECG signal before digital acquisition.

The ECG signal has a small amplitude, usually in the millivolt range, so it must be amplified and filtered before it can be read reliably by an ADC.

The analog front-end of this prototype includes:
1. Power supply 
1. Instrumentation amplifier stage
2. High-pass filter
3. Additional gain stage
4. Low-pass filter
5. Optional 60 Hz notch filtering
6. Output stage for ADC acquisition

## Power supply stage
I've designed to variants, each one for a different voltage supply. I reccomended the version B and it is where I have realized all the tests. More information about the variants A and B can be found in the hardware folder.

## Model A: Virtual Ground Supply

This model uses an op-amp and a voltage divider to create a virtual ground. In my prototype, I used an LM358, but a true rail-to-rail op-amp is recommended for better performance.

<div align="center">
  <img width="700" alt="Virtual ground power supply schematic" src="https://github.com/user-attachments/assets/fc38b19b-4e1c-468f-b17f-4a32d43cc428" />
  <br>
  <sub><b>Figure 1.</b> Virtual ground power supply schematic. Created by Aaron D. Hernández Salmerón, 2026.</sub>
</div>

## Model Limitations

- The maximum current that this model can supply depends on the op-amp, the output buffer stage, and the transistors used.
- The input voltage is divided by two to create the virtual ground reference. For example, if the circuit is supplied with 5 V, the ideal output rails would be approximately +2.5 V and -2.5 V with respect to the virtual ground.
- In practice, the usable voltage range is smaller because of op-amp output swing limitations, transistor voltage drops, diode voltage drops, and the fact that the LM358 is not a true rail-to-rail op-amp.
- This model is useful for low-power analog circuits, but it is not recommended for loads that require high current or high stability.

## Model B: Bipolar Battery Supply

This model uses two equal batteries to create a bipolar power supply, producing positive voltage, ground, and negative voltage rails.

<div align="center">
  <img width="222" alt="Bipolar battery power supply schematic" src="https://github.com/user-attachments/assets/3ecdf94b-0674-48e2-baf5-fe1be13ea59d" />
  <br>
  <sub><b>Figure 2.</b> Bipolar battery power supply schematic. Created by Aaron D. Hernández Salmerón, 2026.</sub>
</div>

Possible battery options include:

- Two 802040 LiPo batteries, 3.7 V, 650 mAh
- Two 18650 Li-ion batteries, 3.7 V, 2200 mAh
- Two 9 V batteries, either Li-ion, LiPo, or alkaline

In my prototype, I used two rechargeable 9 V Li-ion batteries from Steren. Other equivalent battery brands can also be used.

<div align="center">
  <img width="350" alt="Rechargeable 9 V batteries used for bipolar supply" src="https://github.com/user-attachments/assets/e695c606-7a79-4c67-9884-9778116d24c4" />
  <br>
  <sub><b>Figure 3.</b> Rechargeable 9 V batteries used to create a bipolar power supply. Image source: Steren product image, 2026.</sub>
</div>

## Model Limitations

- It is recommended that the voltages of B1 and B2 be as similar as possible.
- If the two batteries have different voltages or discharge at different rates, the positive and negative rails will become unbalanced.
- Battery voltage decreases during use, so the circuit behavior may change over time.
- This model is simple and safer for early testing, but it still does not provide certified medical-grade isolation.

## Warnings for Both Models

I recommend using batteries when testing this prototype on a person.

Dual power supplies, such as PC ATX power supplies or laboratory bench power supplies, can be used for testing the circuit with an arbitrary waveform generator. However, they may introduce noise, instability, and safety risks.

This project does not currently include galvanic isolation, so it must not be connected to a person while powered from a PC ATX supply, bench power supply, oscilloscope-connected setup, or any other non-isolated mains-powered system.

ATX or bench power supplies should only be used for circuit testing with simulated signals, such as those from an arbitrary waveform generator. They are not recommended for tests involving human subjects.

## Input Signal

The input signal comes from surface electrodes placed on the body.

Electrode configuration:

- **RA:** Right arm electrode
- **LA:** Left arm electrode
- **RL:** Right leg reference / bias electrode

The measured signal is the differential voltage between the measurement electrodes.

## Stage 1: Instrumentation Amplifier

The first stage uses an instrumentation amplifier to amplify the small differential ECG signal.

Component used:

```text
Instrumentation amplifier: AD620
```
The AD620 was selected because it is commonly used for low-level differential signal amplification and has good common-mode rejection compared with a simple op-amp differential amplifier. Also AD620 is cheap and its very avalible in Mexico.

The gain of the AD620 is set using an external resistor:

```text
G = 1 + (49.4 kΩ / RG)
```
Current design values:
RG = 5600Ω
Calculated gain = 9.821
Measured gain = [write measured gain here, if available]

## Stage 2: High-Pass Filter

The high-pass filter reduces baseline drift.
Baseline drift may be caused by:
- Respiration
- Body movement
- Electrode polarization
- Slow changes in skin-electrode impedance

The cutoff frequency is calculated using:

```text
fc = 1 / (2πRC)
```

Current design values:

R = [write resistor value]
C = [write capacitor value]
Calculated cutoff frequency = [write value]
Measured cutoff frequency = [write value, if available]

Purpose of this stage:

- Reduce slow baseline movement.
- Keep the ECG waveform centered.
- Improve visualization of the QRS complex.

## Stage 3: Additional Gain Stage

After initial amplification and filtering, an additional gain stage is used to increase the ECG signal amplitude before ADC acquisition.

Current design values:
Op-amp used = TL074
Configuration = Non-inverting
Gain = [write gain value]

For a non-inverting amplifier, the gain is:

G = 1 + (Rf / Rg)

Purpose of this stage:

Increase the ECG signal amplitude.
Match the output voltage range to the ADC input range.
Improve visualization and digital processing.

## Stage 4: Low-Pass Filter

The low-pass filter reduces high-frequency noise.

High-frequency noise may come from:

- Muscle activity
- Electromagnetic interference
- Digital switching noise
- Breadboard parasitic effects
- External electronic devices

The cutoff frequency is calculated using:
```text
fc = 1 / (2πRC)
```

Current design values:

R = [write resistor value]
C = [write capacitor value]
Calculated cutoff frequency = [write value]
Measured cutoff frequency = [write value, if available]

Purpose of this stage:

Reduce high-frequency noise.
Preserve the useful ECG frequency range.
Improve signal quality before ADC conversion.
Optional Stage: 60 Hz Notch Filter

Because the local mains frequency is 60 Hz, the ECG signal may contain strong mains interference.

A notch filter can be used to attenuate this component.

### Current status:

60 Hz notch filter status: Implemented by software

Possible implementations:

- Analog Twin-T notch filter
- Active notch filter
- Digital IIR notch filter in Python
- Digital notch filter in the microcontroller
- Output to ADC

The final analog signal is sent to an ADC or microcontroller input.

## Possible acquisition devices:

- ESP32 internal ADC
- Arduino ADC
- ADS1115 external ADC and arduino, ESP32, STM32, etc
- Other external ADC module and microcontrollers

## Important considerations:

The output voltage must remain within the ADC input range.
If the ADC cannot read negative voltages, the signal must be biased around a reference voltage.
The sampling frequency must be high enough to preserve the ECG waveform.

## Recommended sampling frequency:

250 Hz to 500 Hz
