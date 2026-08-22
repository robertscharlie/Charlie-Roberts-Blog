---
title: "Digital Alarm Clock"
date: 2023-04-01
draft: false
summary: "A 24hr digital alarm clock I designed and built from scratch for a real client brief: a custom-etched PCB, crystal-accurate timekeeping, and a 3D-printed case."
tags: ["electronics", "pcb", "digital-logic", "cad", "embedded-systems", "product-design"]
math: false
image: "images/projects/digital-alarm-clock/front-face.jpg"
category: "Electronics"
highlights:
  - "Crystal-accurate 24hr timekeeping, using a 32kHz oscillator and EEPROM storage"
  - "Etched and soldered the PCB by hand, with its own dedicated voltage regulator"
  - "3D-printed PLA case with laser-cut acrylic faces (190×90×90mm, 500g)"
  - "Designed end-to-end for a real client brief and tested against every spec point"
---

A 24-hour digital alarm clock, designed and built from scratch for a real client brief, from research and spec through to a finished, tested product.

**The brief:** my client needed a desk clock to get him to meetings on time. Accurate, easy to set, readable from across the room, and able to run off mains or battery. That became a spec: under £30, under 1kg, accurate to a minute a week, visible and audible from 3m, two-tone colour scheme.

**Research:** client interviews, a group questionnaire, a mood board, and anthropometric sizing for the buttons and case. I also tore down a few commercial products for ideas: a Dyson vacuum's bold two-tone colour and no-exposed-parts design ethos, a cheap analogue clock's night light and alarm, and Fitbit's legible interfaces. That fed a red-and-black two-tone scheme, filleted edges for grip, and a fully enclosed case.

**Timekeeping:** rather than trust the microcontroller's own clock, a 32kHz crystal oscillator feeds a 4060B ripple counter to divide down to an exact 1Hz pulse. A Genie microcontroller counts those pulses to track time, drives four seven-segment displays through 4026B decade counters, and stores the time and alarm in EEPROM so both survive a power cut.

![Schematic of the two 4026B decade counters driving a pair of seven-segment displays off a 1Hz clock pulse](../../images/projects/digital-alarm-clock/timekeeping-schematic.png)

**Electronics:** three iterations. First, a 555 timer flashing an LED once a second, nowhere near accurate enough. Second, the Genie and seven-segment displays, but the time lived in main memory and was lost on every power cut. Third fixed both: the crystal-and-4060B timebase for accuracy, EEPROM for persistence, a distinct alarm-setting mode, and a battery/mains toggle.

I proved the full circuit on breadboard first: all four displays, the button matrix, and the buzzer running off the real logic, so problems showed up while still easy to rewire.

![The breadboard prototype: all four seven-segment displays wired up and running, reading "18:24"](../../images/projects/digital-alarm-clock/breadboard-prototype.jpg)

Once proven, I laid it out as a custom single-sided PCB and etched it by hand: printing the design onto acetate, exposing a copper-clad board under UV light, developing it, then etching the copper in a heated tank before drilling every hole.

![PCB layout for the timebase board, mirrored for etching, the traces routed around the 4060B counter](../../images/projects/digital-alarm-clock/pcb-layout.png)

![The copper-clad board partway through etching, the printed circuit pattern still visible under the developer solution](../../images/projects/digital-alarm-clock/etching-tank.jpg)

Partway through assembly, the seven-segment displays turned out to need more current than the logic ICs could tolerate at 5V, so I added a second small board: an LM7805 regulator and smoothing capacitor stepping 9V down to a clean 5V, so the whole circuit could still run off one battery.

![Seven-segment displays and PCB wiring mid-assembly, before the case was closed up](../../images/projects/digital-alarm-clock/assembly.jpg)

**Case:** a 3D-printed PLA base (CAD'd in Autodesk Inventor, filleted edges for grip) with laser-cut acrylic front and back faces, sprayed with UV-resistant paint. Both faces are removable for battery access and debugging.

![Rear face of the clock, showing the seven buttons, volume dial, power switch, and DC input](../../images/projects/digital-alarm-clock/back-face.jpg)

**Controls:** seven buttons on the back for clock/alarm setting and a test-buzzer button, a variable resistor for alarm volume, and a mains/battery power switch.

**Testing:** every spec point got measured. 190×90×90mm and 500g, both well inside the limits. Accurate to within a minute over a week with no adjustment. Legible and audible from 3.5m. A focus group changed the battery in 68–93 seconds each, under the 2-minute target.

**What I'd change:** label the back-face buttons, add a logo to the bare side panels, and fix a colour mismatch between the display bezels and case. All feedback from the client and focus group. I also looked at injection moulding as a cheaper-per-unit alternative for a hypothetical production run, at the cost of a much higher tooling setup.

**Stack:** Digital logic (Genie microcontroller), PCB design and etching, Autodesk Inventor (CAD), 3D printing, laser cutting

**Status:** complete

**Full documentation:** [complete design folder (PDF, 53 pages)](../../files/digital-alarm-clock-nea.pdf): research, client interviews, circuit iterations, PCB manufacture diary, and evaluation against every specification point.
