# Radiocontrol — ExpressLRS

## RF Characterization of an ExpressLRS 2.4 GHz Control Link

This section documents the RF observation and characterization of an
ExpressLRS 2.4 GHz control link using HackRF One and Inspectrum.

The objective is to understand what can be observed from the RF layer
without decoding the payload or modifying the drone firmware.

---

## 1. Overview

The FPV drone uses ExpressLRS as its radio-control link.

The experimental setup consists of:

- BetaFPV Meteor75
- ExpressLRS 2.4 GHz
- RadioMaster Pocket
- HackRF One
- Inspectrum

The analysis focuses on:

- RF activity detection
- Temporal periodicity
- Packet-rate characterization
- Frequency behavior
- Frequency hopping
- IQ capture and offline analysis

---

# 2. Hardware

## FPV Drone

BetaFPV Meteor75 equipped with an ExpressLRS 2.4 GHz control link.

![Meteor75](images/hardware/meteor75.jpg)

## Radio Controller

RadioMaster Pocket configured for ExpressLRS 2.4 GHz.

![RadioMaster Pocket](images/hardware/radiomaster-pocket.jpg)

## SDR

HackRF One was used to capture the RF activity as IQ samples.

![HackRF One](images/hardware/hackrf.jpg)

---

# 3. Experimental Architecture

```
RadioMaster Pocket
        │
        │ 2.4 GHz
        ▼
   ExpressLRS Link
        │
        ▼
    Meteor75
```

For RF observation:

```
ExpressLRS RF
      │
      ▼
   HackRF One
      │
      ▼
    IQ Capture
      │
      ▼
   Inspectrum
      │
      ▼
RF Characterization
```
