# 02 — Analog FPV Video: VTX at 5.8 GHz

[← Project overview](../README.md) · [Presentation PDF](../presentation/BEYOND_THE_GOGGLES.pdf)

## Objective

Document the **RF observation and reception of an analog FPV video link** using a HackRF One, a 5.8 GHz RHCP antenna and SDRangel.

The purpose is to characterize a video transmitter (VTX) operating in the 5.8 GHz band and explain the analog video demodulation workflow. **This is not a digital encryption bypass**: the tested VTX transmitted analog video.

## 1. Experimental setup

| Component | Role |
| --- | --- |
| BetaFPV Meteor75 | FPV drone and analog video source |
| Analog 5.8 GHz VTX | RF video transmitter |
| Raceband R3 | Experimental channel |
| **5732 MHz** | R3 center frequency |
| **25 mW** | Reported VTX power setting for this experiment |
| 5.8 GHz RHCP antenna | Reception antenna |
| HackRF One | SDR receiver |
| SDR++ | Spectrum inspection |
| SDRangel | ATV (analog television) demodulation |

The radio-control system (ExpressLRS at 2.4 GHz) is independent of the analog video system at 5.8 GHz. Observing one link does not mean decoding or compromising the other.

## 2. Signal path

```text
Meteor75 camera
      │
      ▼
Analog VTX (Raceband R3, 5732 MHz)
      │
      ▼
5.8 GHz radio transmission
      │
      ▼
RHCP receive antenna
      │
      ▼
HackRF One
      │
      ▼
SDRangel / ATV demodulator
      │
      ▼
Analog video reception
```

## 3. RF observation

The first stage is to tune the SDR near the experimental VTX frequency and inspect the received RF energy and occupied spectrum.

**Experimental reference:** Raceband **R3 — 5732 MHz**.

![5.8 GHz VTX experiment](images/5ghz-poc.jpeg)

![Observed 5.8 GHz spectrum](images/espectro-5ghz.jpeg)

These captures document the experiment. The presence of energy near a frequency alone does not establish that a signal is a specific FPV video transmission; the transmitter configuration and demodulation result provide additional context.

## 4. Analog video demodulation with SDRangel

The documented reception workflow was:

1. Connect the HackRF One and the appropriate 5.8 GHz receiving antenna.
2. Tune the SDR to the configured VTX channel (**5732 MHz**, Raceband R3).
3. Select a sample rate and receive bandwidth appropriate to the available SDR and analog video signal.
4. Configure SDRangel's **ATV demodulator**, including the video standard and demodulation settings suitable for the transmitted signal.
5. Inspect and adjust reception to recover a recognizable image.

SDRangel was used for analog video processing. Video format and demodulator settings may require experimentation; this README does not assert a universally correct PAL/NTSC setting or an exact validated sample rate.

### Why analog reception matters

Unlike an encrypted digital FPV video system, a conventional analog VTX does not require a digital decryption key to display its video. A suitably configured receiver can demodulate a sufficiently strong, compatible signal. Actual reception still depends on antenna polarization, distance, RF conditions, receiver bandwidth and correct demodulator configuration.

## 5. RHCP antenna

**RHCP** means *Right-Hand Circular Polarization*. Antenna polarization is an important factor in the reception chain, especially when matching the receiving antenna to the transmitting system.

An RHCP antenna does not increase the transmitter's output power. Its gain, radiation pattern and polarization influence the received signal and therefore the quality of the video demodulation.

## 6. Security considerations

This experiment illustrates an important distinction between two FPV communication surfaces:

| Surface | Experimental observation | Security interpretation |
| --- | --- | --- |
| ExpressLRS radio control, 2.4 GHz | RF timing and frequency behavior | RF observation is not equivalent to decoding control traffic |
| Analog VTX video, 5.8 GHz | Signal reception and ATV demodulation | Compatible analog video may be viewed without digital decryption |

The experiment demonstrates an **exposure characteristic of analog video transmission**, not an exploit of the flight controller or the ExpressLRS link.

## 7. Limitations

- The observations concern the documented equipment and RF conditions.
- Received image quality depends on antenna, orientation, range, noise and demodulation parameters.
- No digital encryption was broken.
- No claim is made about digital FPV systems such as DJI or HDZero.
- The experiment does not establish the security of every analog or digital VTX implementation.

## 8. Conclusions

- The analog FPV video path was investigated independently from the 2.4 GHz radio-control link.
- The documented VTX operated on **Raceband R3 (5732 MHz)**.
- HackRF One and SDRangel provided an SDR-based workflow for receiving and demodulating analog video.
- Analog reception highlights a potential confidentiality limitation of conventional unencrypted video links.
- Antenna polarization and receiver configuration are critical practical variables.

---

[← Back to Beyond the Goggles](../README.md) · [ExpressLRS research →](../01-radiocontrol-elrs/README.md)
