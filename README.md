# BEYOND THE GOGGLES

### Hacking FPV Drones Through the RF Spectrum

> The drone is more than what we see through the goggles.

**R4Gh0st // Matheo Camacho Vargas**  
Cybersecurity Engineering Student · RF/SDR Research · FPV Pilot  
**Pwn Or Die 2026 — Quito, Ecuador**

---

## About this research

What can we learn about an FPV drone by observing its radio signals?

**Beyond the Goggles** documents hands-on experiments with two independent FPV communication surfaces: the **2.4 GHz ExpressLRS radio-control link** and the **5.8 GHz analog video transmitter (VTX)**. The work focuses on RF observation, IQ capture, signal characterization and analog video reception. It also discusses controlled comparisons between RF equipment and links.

This is an experimental research log, not a claim of universal exploitability or protocol compromise.

![Equipment used in the experiments](01-radiocontrol-elrs/images/materiales.jpeg)

## Experiments

### 01 — ExpressLRS / Radio Control (2.4 GHz)

- Observation of RF activity with transmitter ON and OFF
- HackRF One IQ capture and offline inspection with Inspectrum
- Measured periodicity of approximately **3.968 ms (252 Hz)**, consistent with a configured **250 Hz** link
- Frequency-hopping behavior and limits of identifying a protocol from RF activity alone
- NRF24L01 and HackRF/PortaPack experiments; observations of link quality under specific test conditions

![2.4 GHz experimental evidence](01-radiocontrol-elrs/images/2.4ghz-poc.jpeg)

**[Read the ExpressLRS experiment →](01-radiocontrol-elrs/README.md)**

### 02 — Analog FPV Video / VTX (5.8 GHz)

- RF observation of an analog FPV video transmitter
- Experimental channel **Raceband R3 — 5732 MHz**
- Reception with HackRF One and a 5.8 GHz RHCP antenna
- Video demodulation workflow using SDRangel's ATV demodulator
- Security implications of an analog, unencrypted video link

![5.8 GHz experimental evidence](02-vtx-analog-video/images/5ghz-poc.jpeg)

**[Read the analog VTX experiment →](02-vtx-analog-video/README.md)**

## Experimental setup

| Component | Equipment / software | Purpose |
| --- | --- | --- |
| FPV drone | BetaFPV Meteor75, ELRS 2.4 GHz, analog VTX | Test platform |
| Radio controller | RadioMaster Pocket | ELRS control link |
| SDR | HackRF One + PortaPack H2 | RF observation and experiments |
| 2.4 GHz transceiver | M5Stick-C + NRF24L01 | Dedicated transceiver experiments |
| Antenna | 5.8 GHz RHCP | Analog video reception |
| Signal analysis | Inspectrum, SDR++ | Spectrum and IQ inspection |
| Video demodulation | SDRangel | Analog television (ATV) reception |
| Other tooling | hackrf_transfer, Mayhem | Capture and experimental RF configurations |

## RF architecture

```text
RadioMaster Pocket ── ExpressLRS 2.4 GHz ──> Meteor75
                           │
                           └──> HackRF One / IQ / Inspectrum
                                      (RF observation)

Meteor75 analog VTX ── 5.8 GHz (R3: 5732 MHz) ──> RHCP antenna
                                                     │
                                                 HackRF One
                                                     │
                                                  SDRangel
                                                     │
                                             ATV video reception
```

## Selected findings

| Observation | Interpretation |
| --- | --- |
| Transmitter state correlates with observed 2.4 GHz RF activity | Supports characterization of the test setup; not proof that every observed signal is ELRS |
| ~3.968 ms measured periodicity | Approximately 252 Hz, consistent with the configured 250 Hz ELRS rate; spectrogram lines are not individually counted as packets |
| ELRS link quality decreased in a specific RF experiment, while a tested TBS link did not show a comparable decrease | Condition-specific observation, not proof of universal protocol superiority or immunity |
| Analog video was received through an SDR workflow | The task is RF reception and analog demodulation, **not** breaking digital encryption |

## Presentation

**BEYOND THE GOGGLES — Pwn Or Die 2026, Quito**

[View the presentation (PDF)](presentation/BEYOND_THE_GOGGLES.pdf)

## Research scope and limitations

Experiments were conducted on equipment under the researcher's control. Observed behavior depends on RF conditions, antennas, distance, power, configuration and receiver design. RF activity in a band alone does not identify a protocol, and a single test does not establish a general security property.

The project documents RF characterization and experimental observations. It does not reproduce the ExpressLRS binding/FHSS vulnerability research published by NCC Group in 2022.

## Author

**Matheo Camacho Vargas (R4Gh0st)**  
[GitHub](https://github.com/Ray1172004) · [LinkedIn](https://www.linkedin.com/in/matheo-camacho/)
