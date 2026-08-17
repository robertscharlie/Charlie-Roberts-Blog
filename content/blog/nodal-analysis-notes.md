---
title: "Notes on nodal analysis, before writing a solver"
date: 2026-08-10
draft: false
summary: "Working through modified nodal analysis by hand before turning it into code."
tags: ["circuits", "simulation"]
math: true
---

Before writing any solver code, it's worth working through modified nodal analysis (MNA) by hand. For a resistive network, each node $i$ satisfies Kirchhoff's current law:

$$\sum_{j} \frac{V_i - V_j}{R_{ij}} = I_i$$

which, across the whole circuit, assembles into a linear system:

$$G \cdot V = I$$

where $G$ is the conductance matrix, $V$ is the vector of unknown node voltages, and $I$ is the vector of injected currents.

This is the equation a SPICE-style simulator ultimately solves at every timestep — the rest of the work is bookkeeping: building $G$ from a netlist, handling voltage sources and reactive elements, and stepping the whole thing forward in time.

This post has `math: true` set in its front matter, which loads MathJax only on pages that need it.
