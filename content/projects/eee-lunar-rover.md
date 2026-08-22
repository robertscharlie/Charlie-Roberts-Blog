---
title: "EEE Lunar Rover"
date: 2026-06-30
draft: false
summary: "A WiFi-controlled rover that drives across a mock lunar surface, hunts down artificial rocks, and reads their signals to work out what type each one is."
tags: ["electronics", "pcb", "analogue-electronics", "embedded-systems", "arduino", "wifi", "cad"]
math: false
image: "images/projects/eee-lunar-rover/final-rover.jpg"
category: "Electronics"
highlights:
  - "Awarded Best First Year Project 2026 at Imperial College London"
  - "I designed, tested, and debugged the infrared and magnetic circuits, and soldered three of the four sensor boards"
  - "Built a separate analogue circuit for each of the four sensors, then combined them onto one PCB"
  - "Chose Ackermann steering over differential drive so the rover wouldn't drift when its motors weren't perfectly matched"
  - "Controlled and monitored live from a browser over WiFi"
---

A WiFi-controlled lunar surface rover, built over a summer as ELEC40006 Group 25 at Imperial College London, by six of us: Aila Danish, Hywel Evans, Beiyan Shi, Charles Wu, Saahil Zaki, and me. We designed, built, and tested it, driving across a mocked-up lunar arena, homing in on artificial "rocks," and reading their magnetic, infrared, ultrasound, and RF signatures to classify each one.

**Awarded Best First Year Project 2026 at Imperial College London.**

**My part:** the infrared and magnetic sensing circuits (design, testing, debugging), three of the four sensor boards soldered, and the corresponding report chapters.

## System overview

The base hardware was a shared department kit: an Adafruit Metro M0 Express, a WINC1500 WiFi shield, and an H-bridge motor-driver module giving simple digital/PWM control of motor direction and speed.

![H-bridge motor driver circuit diagram with decode logic, from the module's provided documentation](../../images/projects/eee-lunar-rover/provided-h-bridge.svg)

A laptop-side Flask server bridges browser commands to the board over HTTP and polls it for sensor data. The finished rover is rear-wheel drive with Ackermann front-wheel steering, plus a second servo that swings a combined sensor mount (magnetic, infrared, ultrasound, and RF receivers together) to line up with a rock once the rover is in position.

Each rock embeds a magnet, an IR emitter, an ultrasound transmitter, and an RF transmitter; reading all four classifies it into one of four types:

| Rock type | IR rate | Ultrasound | Magnetic polarity |
| --- | --- | --- | --- |
| Basaltoid | High, λ=547/s | Detected | Down |
| Gravion | Low, λ=312/s | Absent | Down |
| Regolix | Low, λ=312/s | Detected | Up |
| Lunarite | High, λ=547/s | Absent | Up |

The RF signal separately carries the rock's age, UART-encoded onto an 89 kHz carrier:

![RF age signal encoding: idle carrier, then a UART start bit, 8 data bits LSB-first, and a stop bit per character](../../images/projects/eee-lunar-rover/rf-age-encoding.png)

## The four sensing circuits

Each sensor got its own analogue front end (amplification, demodulation, and a Schmitt trigger), built mostly around the MCP6022 op-amp and proven on breadboard first.

![Debugging a sensor front end on the bench: oscilloscope, breadboard prototype, and a rock's internal magnet and RF coil](../../images/projects/eee-lunar-rover/lab-bench-testing.jpg)

**Infrared:** the rock fires 50 µs pulses at a Poisson rate of 312/s or 547/s. Our first phototransistor (SFH300) picked up ordinary room light as strongly as the rock's 950 nm signal and had too narrow an acceptance angle for light scattered inside the rock; the TEFT4300 fixed both. A high-pass filter strips DC/mains flicker, two non-inverting gain stages (~121× total) recover the signal, and a Schmitt trigger cleans it up. Firmware counts pulses over a 3-second window and classifies against a 430/s threshold, six standard deviations from both rock populations, so misclassification is essentially impossible.

**Magnetic:** needed field *direction*, not just presence, so we used the A1324 linear Hall sensor rather than a digital switch, and amplified the small deviation from a ~2.5 V reference with a ~47× differential stage. A few millivolts of real-world mismatch between sensor and reference becomes nearly a volt at that gain, which is why the three output states (up ~3.1 V, down ~0 V, unknown ~2.3 V) aren't perfectly symmetric but are cleanly separated.

**Ultrasound:** the Prowave 400SR100 receiver picks up a weak 40 kHz tone through the rock's 1 mm acoustic window. The first op-amp stage acts as a transimpedance front end, converting the piezo's current output straight to voltage; a slow RC rectifier (~47 ms) then reduces the signal to a stable presence/absence level. Detectable up to about 10 cm from the rock.

**RF:** picked up purely by magnetic coupling from a coil embedded in the rock's own PCB, no electrical connection at all. Our hand-wound coil (432 µH) forms a resonant LC tank tuned to 89 kHz (measured Q ≈ 25) to boost the signal without smearing the UART bits riding on it. Our first op-amp choice (LT1366) didn't have enough gain-bandwidth or slew rate at 89 kHz; the MCP6022 did. Two ×10 stages amplify the carrier, an envelope detector recovers the UART waveform, and a Schmitt trigger cleans it up. The finished chain correctly decoded a rock reporting "#933": 9.33 billion years old.

![RF transmitter prototyped on breadboard for testing, wired to a hand-wound coil and the rock's own battery/switch/button assembly](../../images/projects/eee-lunar-rover/rf-transmitter-breadboard.jpg)

## PCB and build

All four circuits were laid out together as a two-layer board in KiCad, with a shared ground plane and socketed op-amps for easy swapping. It wasn't ordered and populated in time, though: the rover that actually shipped ran each circuit soldered onto its own Veroboard module instead, a compromise between permanent breadboard and a fabricated board we couldn't iterate on.

![3D-rendered view of the designed sensor PCB, showing IC sockets, connectors, and passive component placement across all four circuits](../../images/projects/eee-lunar-rover/sensor-pcb-3d.png)

## Chassis and drivetrain

Rear-wheel drive with front-wheel Ackermann steering, chosen over the lab kit's differential steering because differential drive couples turning to motor speed and drifts whenever the two motors aren't perfectly matched. A single rear motor drives through a gear train; one servo steers the front wheels, a second independently swings the sensor mount to fine-align with a rock. The 3D-printed chassis (steering geometry adapted from Tim Hanewich's open-source [PYPER2](https://github.com/TimHanewich/PYPER2)) came in at 512 g, under the 750 g limit.

![CAD render of the full chassis assembly: steering linkage, wheels, and mounting points for the sensor board](../../images/projects/eee-lunar-rover/chassis-cad.jpg)

## Software and control

Firmware on the Metro M0 runs a cooperative, poll-driven loop rather than an RTOS: each subsystem is time-sliced with `millis()` checks, which sidesteps re-entrancy issues since the HTTP server needs unbounded blocking time to read requests. We evaluated migrating to an ESP32 for more headroom and native WiFi, but the change came too late to fully validate, so it stayed a backup.

A Flask server on the laptop bridges the browser to the rover's onboard HTTP server across three pages: a landing page, WASD manual control (Q/E for the sensor mount, R to reset it), and a live telemetry page mirroring the classification table above.

![Detection page: live magnetic, infrared, ultrasound, and RF readings feeding the rock classification, plus a saved detection history](../../images/projects/eee-lunar-rover/web-detect.jpg)

All four readings feed one truth-table lookup, where each rock type is a unique combination of IR rate, ultrasound, and magnetic polarity. It's duplicated in the Flask bridge so the front end stays sensible even if the Arduino omits `rock_type`.

## Testing

Testing happened on a purpose-built lunar-regolith-textured arena with painted foam rocks, several rovers running at once during interim sessions:

![Overhead view of the test arena: painted foam rocks and multiple rovers navigating the surface at once](../../images/projects/eee-lunar-rover/rock-arena.jpg)

**Team:** Aila Danish, Hywel Evans, Charlie Roberts, Beiyan Shi, Charles Wu, Saahil Zaki (ELEC40006 Group 25).

**Stack:** Adafruit Metro M0 Express (Arduino), WINC1500 WiFi, KiCad, Python/Flask, Autodesk Fusion 360 (CAD), 3D printing.

**Status:** complete. Final report submitted June 2026.

**Repo:** private (Imperial College coursework repository).
