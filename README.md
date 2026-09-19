# BEYOND THE GOGGLES

## Hacking FPV Drones Through the RF Spectrum

> RF observation, characterization and analysis of FPV drone communication links.

**R4Gh0st // Matheo Camacho Vargas**

SOC Operator · Cybersecurity Engineering Student · FPV Pilot

Pwn Or Die 2026 — Quito, Ecuador

---

## Overview

What can we learn about an FPV drone simply by observing its radio signals?

This project explores the RF attack surface of a modular FPV drone through
passive observation, signal characterization and analog video reception.

The research focuses on:

- 2.4 GHz ExpressLRS control link
- Frequency-hopping behavior
- RF characterization using HackRF and Inspectrum
- 2.4 GHz experimentation using NRF24L01
- SDR analysis using HackRF One
- Analog FPV video at 5.8 GHz
- Video reception and demodulation using SDRangel
- RHCP antenna usage for 5.8 GHz reception
- Experimental comparison of different RF links

---

# 1. Hardware

## FPV Platform

- BetaFPV Meteor75
- ExpressLRS 2.4 GHz
- Analog 5.8 GHz VTX

## RF Equipment

- HackRF One
- PortaPack H2
- M5Stick-C
- NRF24L01
- 5.8 GHz RHCP antenna

## Radio

- RadioMaster Pocket
- ExpressLRS 2.4 GHz

---

# 2. Software

- Inspectrum
- SDRangel
- SDR++
- hackrf_transfer
- Mayhem

---

# 3. RF Architecture

The FPV platform exposes multiple communication surfaces.

### Control Link

ExpressLRS:

    2.4 GHz

### Video Link

Analog FPV:

    5.8 GHz

The objective is not to classify one architecture as secure or insecure,
but to understand how different communication surfaces can be observed
and characterized independently.

---

# 4. ExpressLRS RF Characterization

The first experiment focused on observing the 2.4 GHz control link.

The HackRF was used to capture IQ samples and Inspectrum was used for
offline analysis.

The transmitter was tested in two states:

    Transmitter OFF
        ↓
    Characteristic RF activity disappears

    Transmitter ON
        ↓
    RF activity appears

This provided an initial correlation between the transmitter state and
the observed RF activity.

---

# 5. Packet Rate Measurement

The captured IQ data was analyzed using Inspectrum.

Observed values:

    Period ≈ 3.968 ms
    Rate ≈ 252.016 Hz

The relationship between period and rate is:

    Rate = 1 / Δt

Using approximately 4 ms:

    1 / 0.004 ≈ 250 Hz

The measured periodicity is therefore consistent with the configured
250 Hz packet rate of the experimental ExpressLRS link.

### Important clarification

The visible lines in the spectrogram are not interpreted individually
as packets.

The measurement is based on the periodicity calculated by Inspectrum,
rather than counting the visible lines.

---

# 6. Frequency Hopping

ExpressLRS uses frequency hopping.

The observed RF activity changes position in frequency over time.

The experiment focuses on characterizing this behavior from the RF
perspective without decoding the payload.

Finding activity in the 2.4 GHz band alone does not prove that a signal
is ExpressLRS.

Other systems such as Wi-Fi, Bluetooth, other RC links and ambient noise
can also occupy this spectrum.

Characterization therefore considers:

- Frequency
- Bandwidth
- Duration
- Periodicity
- Frequency behavior
- Signal-to-noise conditions

---

# 7. HackRF and Inspectrum

The analysis chain was:

    RF signal
        ↓
    HackRF One
        ↓
    IQ samples
        ↓
    hackrf_transfer
        ↓
    IQ capture
        ↓
    Inspectrum
        ↓
    RF characterization

HackRF provides the RF capture.

Inspectrum provides the analysis of the captured IQ data.

---

# 8. NRF24L01 Experiment

An M5Stick-C with an NRF24L01 was used to experiment with RF activity
within the 2.4 GHz band.

The NRF24L01 is a specialized RF transceiver with discrete frequency
positions.

This experiment was used to compare a dedicated RF transceiver with
the flexibility provided by an SDR platform.

---

# 9. HackRF + Mayhem

The second experimental platform used:

    HackRF One + PortaPack H2 + Mayhem

The main conceptual parameters are:

    WIDTH → configured frequency space
    TYPE  → signal type
    HOP   → movement between frequency segments
    TX    → transmission duration
    SLEEP → pause
    JITTER → timing variation
    GAIN  → gain

A simple way to remember the concept:

> WIDTH defines the space;
> TYPE defines the nature of the signal;
> HOP defines the movement.

The objective of the experiment was to compare different RF architectures
and observe how the FPV link behaved under the tested conditions.

---

# 10. Experimental Comparison

The M5Stick/NRF24 and HackRF/Mayhem configurations do not represent the
same RF architecture.

The observed behavior can depend on multiple variables:

- Frequency
- Bandwidth
- Received power
- Distance
- Antenna
- Orientation
- Receiver sensitivity
- Protocol processing
- Timing
- Receiver diversity

Therefore, the observed results are treated as experimental results
under specific laboratory conditions rather than universal behavior.

---

# 11. Analog FPV Video

The second major part of the project focuses on the analog video link.

The experimental chain is:

    Meteor75
        ↓
    Analog VTX
        ↓
    5.8 GHz RF
        ↓
    RHCP antenna
        ↓
    HackRF One
        ↓
    SDRangel
        ↓
    ATV Demodulator
        ↓
    Recovered video

The experimental VTX channel used was:

    R3 — 5732 MHz

Because the system uses analog video, the objective is to receive and
demodulate the signal rather than decrypt a digital payload.

---

# 12. RHCP Antenna

RHCP means:

    Right-Hand Circular Polarization

The antenna is used to provide suitable polarization and reception
characteristics for the 5.8 GHz FPV signal.

The antenna does not magically increase the transmitter power.

Its characteristics such as polarization, gain and radiation pattern
affect how the receiver receives the RF signal.

The objective is to provide better reception conditions for the SDR and
therefore facilitate analog video demodulation.

---

# 13. SDRangel

SDRangel was used to process the captured 5.8 GHz analog FPV signal.

The general workflow is:

    Tune to the VTX frequency
        ↓
    Configure the appropriate sample rate
        ↓
    Configure ATV demodulation
        ↓
    Recover the analog video

---

# 14. ELRS vs TBS Experiment

A separate experiment compared the behavior of an ELRS link and a TBS
link under the tested RF conditions.

The observation was:

> Under the conditions of the experiment, ELRS showed an LQ reduction
> while TBS did not show a comparable reduction.

This result does not establish universal immunity or superiority.

Potential variables include:

- Transmit power
- Frequency
- Antennas
- Receiver architecture
- Sensitivity
- Diversity
- Protocol
- Distance
- Geometry
- Timing

The result should therefore be interpreted as an experimental observation
under specific conditions.

---

# 15. Security Research Context — NCC Group

NCC Group published research in 2022 concerning vulnerabilities in
ExpressLRS 1.x and 2.x related to the binding process and generation of
the frequency-hopping sequence.

This research provides an interesting connection between RF observation
and protocol security.

The project presented here does not reproduce that attack.

Instead, the focus is on the first stage:

    Observe
       ↓
    Characterize
       ↓
    Understand
       ↓
    Investigate

The NCC Group research demonstrates how RF/protocol analysis can become
relevant to security research beyond simple spectrum observation.

---

# 16. Commands

## HackRF capture

```bash
hackrf_transfer \
  -r capture.iq \
  -f <CENTER_FREQUENCY> \
  -s <SAMPLE_RATE>
