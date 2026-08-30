---
title: "Pygame Games"
date: 2023-09-11
draft: false
summary: "A small collection of games built in Pygame while I was learning Python and game dev: a brick-breaker and a from-scratch Tetris."
tags: ["python", "pygame", "game-dev", "2d-graphics"]
math: false
image: "images/projects/pygame-games/pygame-logo.png"
category: "Game"
link: "https://github.com/robertscharlie/Pygame-Games"
linkLabel: "Repo"
highlights:
  - "Ballz: an aim-and-fire brick-breaker with combo scoring and coin pickups"
  - "Tetris: full piece rotation and row-clearing, built entirely from scratch"
---

A small collection of games built with Pygame while learning Python and game development. Each one was a way to practise a different concept: sprite groups, collision detection, grid logic, and simple physics.

**Ballz:** a brick-breaker inspired by the mobile game *Ballz*. Click to aim and fire a stream of balls at descending numbered blocks; each hit knocks a block down, destroying it at zero. Catch a coin mid-flight for an extra ball next volley. Let a block reach the bottom and it's game over.

![Ballz gameplay, a wave of numbered blocks descending towards the launcher](../../images/projects/pygame-games/ballz.png)

Balls launch 200ms apart so a volley reads as a stream rather than a clump, each tracking its own position as a float for sub-pixel precision. Volley speed scales with score, and each new volley launches from wherever the last ball came to rest rather than resetting to centre, so aim carries over between shots.

![Ballz mid-volley, an in-progress run, captured straight from the actual game code](../../images/projects/pygame-games/ballz-inplay.png)

![Ballz game over, a block has crossed the bottom of the play area, ending the run](../../images/projects/pygame-games/ballz-gameover.png)

**Tetris:** a classic falling-block puzzle built from scratch, with piece rotation and full-row clearing. Arrows shift and drop the piece, up rotates it, and clearing rows scores points.

![Tetris gameplay, a partial board with the score shown in the side panel](../../images/projects/pygame-games/tetris.png)

There's no piece/entity system: the board is a 2D grid of single-character cell states, and falling, locking, and clearing all shift values directly around that grid. Each piece is a 3×3 grid of the same characters, which makes rotation almost free, just transposing and reversing the slice. Fall speed is time-based rather than frame-based, so it stays consistent regardless of frame rate.

![Tetris mid-game, a rotated piece falling above a locked row, captured from the actual game code](../../images/projects/pygame-games/tetris-inplay.png)

![Tetris game over, the stack has filled up to the top of the board and no new piece can spawn](../../images/projects/pygame-games/tetris-gameover.png)

**Stack:** Python, Pygame

**Status:** ongoing collection

**Repo:** [github.com/robertscharlie/Pygame-Games](https://github.com/robertscharlie/Pygame-Games)
