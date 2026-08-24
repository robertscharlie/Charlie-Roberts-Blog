---
title: "Operational Amplifier in LTSpice"
slug: "discrete-bjt-opamp"
date: 2026-03-15
draft: false
summary: "What's actually inside an op-amp? I spent a few months teaching myself, then built one from scratch out of individual transistors in LTSpice to find out."
tags: ["electronics", "analogue-electronics", "bjt", "op-amps", "ltspice", "simulation"]
math: false
image: "images/projects/discrete-bjt-opamp/full-schematic.png"
category: "Electronics"
highlights:
  - "Learned and built every stage myself: differential pair, current mirrors, and a Darlington output stage"
  - "Tested it by stepping the feedback resistor through four values and checking the gain against theory"
  - "Measured gain matched the predicted Rf/Rin ratio almost exactly, even at ×1000"
---

Op-amps are the workhorse of analogue electronics, and normally you just reach for one as a chip. I wanted to know what was actually going on inside, so over a few months of teaching myself the theory I built one from individual transistors in LTSpice instead: no ICs, just BJTs, resistors, and a lot of simulation runs.

An op-amp has to amplify the difference between two inputs while ignoring anything common to both, then supply enough current to drive a real load. That splits into a few distinct jobs, and I learned each one as its own stage before wiring them together:

**Differential pair:** two transistors sharing a tail current, taking the two inputs and turning their difference into a single signal. This is the front end that gives an op-amp its defining behaviour.

**Current mirrors:** matched transistor pairs, used in place of plain resistors to set the diff pair's tail current and to act as its load. A current-mirror load gives far more gain than a resistor would, which is most of where the op-amp's overall gain actually comes from.

**Darlington output stage:** one transistor driving the base of a second multiplies the current gain, so the final stage can push real current into a load instead of just a simulation probe.

Building it stage by stage and getting each one working before adding the next is how the theory actually clicked. Reading about a current mirror is one thing; watching it hold a tail current steady on a simulated trace is another.

![Full LTSpice schematic: seventeen transistors (Q1-Q17) split between 2N2907 PNPs and 2N2222 NPNs, running off a split ±5V supply](../../images/projects/discrete-bjt-opamp/full-schematic.png)

The finished circuit runs to seventeen transistors: 2N2907 PNPs for the input differential pair and the upper current mirrors, 2N2222 NPNs for the tail source and the Darlington output. It's biased off a split ±5V supply rather than a single rail, which is what lets the output swing through 0V like a real op-amp instead of sitting offset. R1 and R3 (8.6kΩ and 9.3kΩ) set the mirror currents that establish the open-loop gain, a 1nF cap across the second stage keeps it from oscillating once the feedback loop closes, and a pair of 12Ω resistors on the output stage limit the current the Darlington pair can dump into a short.

## Checking the gain

To prove the finished amplifier actually behaved like an op-amp, rather than just a pile of transistors that happened to simulate, I built a simple test bench: the amplifier wired as an inverting stage, input resistor fixed at 1kΩ, and the feedback resistor stepped through 1k, 10k, 100k, and 1M across the same AC sweep.

![LTSpice AC-analysis test bench: the op-amp in an inverting configuration, with the feedback resistor stepped across 1k, 10k, 100k, and 1M for the sweep below](../../images/projects/discrete-bjt-opamp/ac-test-bench.png)

For an inverting amplifier the textbook gain is just the resistor ratio, Rf/Rin, so each feedback value has a clear number to check against: 1k should give ×1 (0 dB), 10k should give ×10 (20 dB), 100k should give ×100 (40 dB), and 1M should give ×1000 (60 dB). Reading the flat, low-frequency part of each curve off the plot and converting it back from dB to a linear value lined up with those predictions almost exactly. Only the 1M case came in a touch low, around 59 dB instead of 60, which is the amplifier's own finite open-loop gain starting to limit how much closed-loop gain it can actually deliver.

![Resulting frequency response: closed-loop gain holding flat at low frequency, then rolling off well before 1 MHz for every feedback resistor value tested](../../images/projects/discrete-bjt-opamp/gain-plot.png)

The same plot shows the usual gain-bandwidth trade-off too: the higher the feedback resistor pushes the gain, the earlier it rolls off, since gain and bandwidth are linked by the same finite gain-bandwidth product in any real amplifier, discrete or otherwise.

**Stack:** LTSpice, discrete BJTs

**Status:** complete
