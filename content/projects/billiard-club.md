---
title: "Billiard Club"
date: 2024-10-22
draft: false
summary: "A full 2D pool game built from scratch in Python, complete with its own physics engine, an AI opponent, and a full menu system."
tags: ["python", "pygame", "game-dev", "physics-engine", "2d-graphics"]
math: false
image: "images/projects/billiard-club/main-menu.png"
category: "Game"
link: "https://github.com/robertscharlie/billiardclub-NEA"
linkLabel: "Repo"
highlights:
  - "Full 8-ball rules: turn tracking, fouls, potting, win detection"
  - "Wrote my own physics engine for ball-to-ball and ball-to-wall collisions"
  - "Bank-shot aim assist that projects the cue ball's path off the rails"
  - "A basic AI opponent, plus Player vs Player and Sandbox modes"
---

A 2D pool game built from scratch in Python and Pygame, with its own physics engine, an AI opponent, and a full menu system. Built solo over eight stages, from a bouncing-ball prototype to final polish.

It implements full 8-ball rules: turn tracking, spots/stripes assignment, fouls, and win detection, on top of a physics engine I wrote from first principles: friction, impulse-based collisions, and potting detection against each hole. An aim-assist line shows the cue ball's projected path, reflecting off the cushion if it would bank first. There's a basic AI opponent for Player vs AI games, plus Player vs Player and a free-play Sandbox mode.

![Customise Game screen, where each player enters a name and picks a colour off a rainbow slider, which then colours their turn indicator and ball tracker in-game](../../images/projects/billiard-club/customise-game.png)

Around that sits a full menu system: main menu, mode select, a "Customise Game" screen where each player picks a name and colour, a pause menu, and win/lose screens. Whichever colour a player picks carries through into the match, tinting their name and ball-tracker border so it's clear at a glance whose shot it is. Volume splits into master, music, and sound-effect sliders, with audio pulled from freesound.com.

![Mid-game, showing the aim line, shot power slider, turn indicator, and per-player ball tracker along the bottom](../../images/projects/billiard-club/gameplay-break.png)

I built it in stages, testing after each one: physics and collisions first, then the table and UI, then cue mechanics, rules, AI, menus, audio, and a final polish pass. The trickiest parts got prototyped in isolation first: a standalone `ballPhysics.py` for collisions and restitution, and a `sliders.py` for the shot-power UI, before merging both into `main.py`.

![End-of-game screen showing the win message, final ball positions, and the potted/remaining ball tracker for each player](../../images/projects/billiard-club/win-screen.png)

**Testing:** a full pass/fail test plan covering rules, UI, and menus, plus stress-testing the physics with a screen full of balls and getting friends to play through it.

**What I'd add next:** a short tutorial overlay for first-time players, and a 9-ball mode. I sketched the logic for both but ran out of time to build them.

**Controls:** aim with the mouse, set shot strength with the power slider, left-click to shoot, Esc to pause.

**Stack:** Python, Pygame, NumPy, OpenCV

**Status:** complete

**Repo:** [github.com/robertscharlie/billiardclub-NEA](https://github.com/robertscharlie/billiardclub-NEA)

**Full write-up:** [complete design document (PDF, 287 pages)](../../files/billiard-club-nea.pdf): analysis, design, the full staged build log, testing, and evaluation.
