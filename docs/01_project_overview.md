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
