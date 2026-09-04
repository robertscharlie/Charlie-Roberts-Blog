---
title: "ESP32 Vitals Monitor"
slug: "vitals-monitor"
date: 2026-09-04
draft: false
summary: "An ESP32 reading heart rate, SpO2, body temperature, and a single-lead ECG, serving its own live dashboard over WiFi from a self-hosted access point; no router, app, or internet connection needed."
tags: ["electronics", "embedded-systems", "esp32", "arduino", "biosensing", "wifi"]
math: false
image: "images/projects/vitals-monitor/full-build.jpg"
category: "Electronics"
link: "https://github.com/robertscharlie/esp-vitals-monitor"
linkLabel: "Repo"
highlights:
  - "A self-contained vitals monitor: heart rate, SpO2, body temperature, and a single-lead ECG all read from one ESP32"
  - "Serves its own live dashboard straight from the board over WiFi, no router, app, or internet connection needed"
  - "Plots a live, adjustable ECG waveform on the dashboard, not just a single heart-rate number"
  - "Timestamps every ECG sample as it arrives in the browser, so the trace stays accurate even if the WiFi send rate drifts"
---

An ESP32-based vitals monitor that reads heart rate, SpO2, body temperature, and a single-lead ECG. A MAX30102 (heart rate and SpO2) and an MLX90614 (body temperature) share one I2C bus, with an AD8232 ECG front end sampled on its own ADC pin. Rather than joining a home network, the ESP32 starts its own WiFi access point and serves a live dashboard itself over a WebSocket, so it can be checked from a laptop or phone browser at `192.168.4.1` with no router, app, or internet connection needed.

![Dashboard paused on two beats, gain and offset pulled in to read a small signal clearly](../../images/projects/vitals-monitor/dashboard-paused.jpg)

**Electrode placement.** The three ECG pads went on like this: red (RA) below the right collarbone, yellow (LA) below the left collarbone, and green (RL, the reference) on the lower left side of the torso, over a rib rather than muscle. Skin was wiped with an alcohol pad and left to dry before sticking each pad down; contact quality mattered far more than getting the placement exact to the millimetre.

![The three ECG electrode pads before being attached](../../images/projects/vitals-monitor/ecg-electrodes.jpg)

## Hardware and wiring

| Part | Notes |
| --- | --- |
| ESP32 dev board | |
| MAX30102 breakout | Pulse oximeter, I2C |
| MLX90614 breakout | Infrared body temperature sensor, I2C |
| AD8232 breakout | Single-lead ECG front end |
| ECG electrode pads ×3 | Disposable, snap-lead |
| Breadboard + jumper wires | |

| Signal | GPIO |
| --- | --- |
| MAX30102 SDA | 26 |
| MAX30102 SCL | 27 |
| MLX90614 SDA | 26 (shares the I2C bus with the MAX30102) |
| MLX90614 SCL | 27 |
| AD8232 OUTPUT | 34 |
| AD8232 LO+ | 35 |
| AD8232 LO- | 32 |

The MAX30102 and MLX90614 share the same I2C bus, running at `I2C_SPEED_STANDARD` (100kHz) rather than 400kHz: the extra capacitance and noise from the breadboard jumper wiring made the MAX30102's `begin()` reliably fail at 400kHz, even though both sensors ACKed fine once running. The AD8232's SDN pin was left unconnected, since it has its own onboard pull-up, and LO+/LO- got no external pull resistors, since the AD8232 drives them itself.

![Wiring close-up: MAX30102, AD8232 (heart LED lit) and MLX90614](../../images/projects/vitals-monitor/wiring-closeup.jpg)

## Firmware

Built on `ESPAsyncWebServer` and `AsyncTCP` (the ESP32Async/mathieucarbou fork, since the older forks wouldn't compile against this ESP32 Arduino core), the SparkFun MAX3010x library, and the Adafruit MLX90614 library.

The ECG lead-off pins get debounced before being trusted, since a single noisy read would otherwise flicker the "lead off" warning constantly:

```cpp
if (rawOff == leadOffRawLast) {
  if (leadOffStableCount < LEADOFF_DEBOUNCE) leadOffStableCount++;
} else {
  leadOffRawLast = rawOff;
  leadOffStableCount = 0;
}
if (leadOffStableCount >= LEADOFF_DEBOUNCE && rawOff != leadOffState) {
  leadOffState = rawOff;
  // ...send the change over WebSocket
}
```

The dashboard also timestamps each ECG sample as it arrives in the browser instead of assuming a fixed 250Hz, so the timebase stays accurate even if the ESP32's real send rate drifts slightly under load, since a slow I2C read or WiFi send can stall the loop for a couple of milliseconds.

## Using the dashboard

Heart rate, SpO2, and temperature update live alongside the ECG trace, and four controls shape how that trace reads. Timebase sets how many seconds of ECG are shown across the screen, from 0.5s to 10s. Gain zooms the vertical scale in or out around the baseline, deliberately not auto-fit, since that would stretch a weak signal to fill the screen on its own; it has to be pulled in manually, giving an honest reflection of how big the real signal actually is. Y offset pans the trace up or down if it's sitting near the top or bottom of the panel. Pause and resume freezes the display for a closer look: timebase, gain, and offset stay adjustable while paused, and data keeps arriving in the background, so resume jumps straight back to live instead of catching up.

![Dashboard at a wider 7.5s timebase showing several beats in a row](../../images/projects/vitals-monitor/dashboard-wide-timebase.jpg)

## Limitations

This isn't a medical device, it's a coursework and hobby project. The SpO2 reading uses the standard empirical ratio-of-ratios formula and isn't calibrated against a reference oximeter, and the ECG is a single unfiltered lead with no clinical-grade signal processing, so all three readings should be treated as indicative only, not diagnostic.

**Controls:** timebase, gain, and Y offset shape the ECG trace; pause and resume freezes the display without stopping data collection underneath.

**Stack:** ESP32 (Arduino), ESPAsyncWebServer, AsyncTCP, SparkFun MAX3010x, Adafruit MLX90614

**Status:** complete (prototype)

**Repo:** [github.com/robertscharlie/esp-vitals-monitor](https://github.com/robertscharlie/esp-vitals-monitor)
