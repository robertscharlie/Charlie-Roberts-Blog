---
title: "Circuit Builder"
date: 2026-08-14
draft: false
summary: "A desktop circuit editor and simulator I built myself: sketch a schematic, wire it up, and watch it actually work in DC, transient, and frequency response."
tags: ["python", "pyside6", "electronics", "circuit-simulation", "desktop-app", "gui"]
math: false
image: "images/projects/circuit-builder/01_canvas_overview.png"
category: "Software"
link: "https://github.com/robertscharlie/Circuit-Builder"
linkLabel: "Repo"
highlights:
  - "Wrote my own nodal analysis solver for DC, transient, and AC, with no NumPy or SciPy"
  - "Live Simulate mode shows the voltage at every terminal, updating as you edit the circuit"
  - "Frequency response and Bode plots, with the sweep range guessed from the circuit's own values"
  - "Full undo/redo, wire splitting and tapping, and a plain-JSON save format"
---

A desktop app for sketching circuit schematics and simulating them, built with Python and PySide6 (Qt). Drop components onto a canvas, wire them up, and either watch the voltages settle live or sweep a frequency response.

Components (resistor, battery, capacitor, inductor, bulb, switch, and a bare node) go down with a shortcut key or a sidebar click, into a placement mode where a ghost follows the cursor until you click to drop it. Wiring is drag-between-terminals; dropping a wire on an existing one taps into it, and landing both terminals on the same wire splices a component in directly. Everything goes through undo/redo, including renames and revalues.

**Simulate** (`F5`) runs a live DC solve and labels every terminal's voltage as you edit. Capacitors and inductors charge and discharge over real time rather than snapping to steady state, so opening a switch after current has built up in an inductor makes the whole network swing negative for a moment as the kickback plays out.

![Live simulation, with voltage labelled at every terminal, refreshing as the circuit is edited](../../images/projects/circuit-builder/02_simulate_live.png)

![Inductor kickback caught mid-swing: the switch has just opened and the loop reads -11.9V against the 9V source, before it decays back toward 0](../../images/projects/circuit-builder/03_inductor_kickback.png)

**Frequency Response** opens a Bode plot panel where you pick the driving source and probe node and flip between magnitude and phase. The sweep range is guessed automatically from the circuit's own component values, and the panel spells out what's being varied and what the plotted values are relative to.

![Frequency response on a high-pass filter, magnitude rolling off toward the low end, with the swept input/output explained above the plot](../../images/projects/circuit-builder/04_frequency_response_highpass_magnitude.png)

![The same high-pass filter's phase response, on the Phase tab of the same panel](../../images/projects/circuit-builder/05_frequency_response_highpass_phase.png)

The same panel works just as well on a series RLC circuit, though the result looks nothing like the highpass roll-off above: with the resistor, inductor, and capacitor all in series, the two reactances cancel out exactly at the resonant frequency, so the voltage measured between them collapses to almost nothing right at resonance rather than peaking. It shows up as a sharp notch rather than a bump, dropping over 140dB before recovering either side of it.

![Frequency response of a series RLC circuit, showing a sharp resonant notch rather than a peak, where the inductor and capacitor's reactances cancel each other out](../../images/projects/circuit-builder/07_rlc_resonance.png)

Save/open is plain JSON, and a handful of worked examples ship in `example_files/` (RC and RLC circuits, a voltage divider, parallel resistors, and a few basics circuits), so the app doesn't open onto an empty canvas. There's also zoom/pan and a full shortcut reference under Help > Controls (`F1`).

![The Help > Controls reference panel, listing shortcuts for placing components, wiring, moving things, and every right-click menu](../../images/projects/circuit-builder/06_shortcut_reference.png)

Underneath the UI, `core/` is deliberately Qt-free: `circuit_model.py` holds the plain-data circuit representation and JSON save/load, `simulation.py` does DC and transient nodal analysis, and `ac_simulation.py` generalises it to complex-valued nodal analysis for AC: resistors as a real admittance, capacitors as `jwC`, inductors as `1/(jwL)`. No NumPy or SciPy; the linear algebra is hand-written using Python's own `cmath`.

**Testing:** a large set of standalone assertion scripts rather than a pytest suite, covering the solvers, wire splitting/tapping, undo/redo, and UI interactions, runnable together or individually.

**What's next:** a proper ground symbol (0V currently defaults to the first battery's negative terminal), current readouts, multi-select with copy/paste, right-angle wire routing, and a packaged executable.

**Controls:** place a component with its shortcut key or sidebar, drag between terminals to wire, `F5` to simulate, `F1` for the shortcut reference.

**Stack:** Python, PySide6 (Qt), Matplotlib

**Status:** in progress

**Repo:** [github.com/robertscharlie/Circuit-Builder](https://github.com/robertscharlie/Circuit-Builder)
