# Analog Front-End

## Overview

The analog front-end is responsible for conditioning the ECG signal before digital acquisition.

The ECG signal has a small amplitude, usually in the millivolt range, so it must be amplified and filtered before it can be read reliably by an ADC.

The analog front-end of this prototype includes:

1. Instrumentation amplifier stage
2. High-pass filter
3. Additional gain stage
4. Low-pass filter
5. Optional 60 Hz notch filtering
6. Output stage for ADC acquisition

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
RG = [write your resistor value here]
Calculated gain = [write calculated gain here]
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

Current status:

60 Hz notch filter status: Implemented by software

Possible implementations:

Analog Twin-T notch filter
Active notch filter
Digital IIR notch filter in Python
Digital notch filter in the microcontroller
Output to ADC

The final analog signal is sent to an ADC or microcontroller input.

## Possible acquisition devices:

ESP32 internal ADC
Arduino ADC
ADS1115 external ADC and arduino, ESP32, STM32, etc
Other external ADC module

## Important considerations:

The output voltage must remain within the ADC input range.
If the ADC cannot read negative voltages, the signal must be biased around a reference voltage.
The sampling frequency must be high enough to preserve the ECG waveform.

## Recommended sampling frequency:

250 Hz to 500 Hz
