# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file web game: `tictactoe.html`. No build step, no dependencies, no package manager. Open the file directly in a browser to play.

## Git workflow

After every meaningful change: commit with a descriptive message and push to `origin/master`.

```bash
git add tictactoe.html
git commit -m "subject line\n\nbody if needed"
git push
```

GitHub repo: https://github.com/castrotino102-coder/tic-tac-toe

## Architecture

Everything lives in `tictactoe.html` in three sections:

- **CSS** (`<style>`) — dark navy theme (`#1a1a2e` / `#16213e`), red accent `#e94560`, blue accent `#1e90ff`. Animations: `.pop` on placement, `.pulse` on win cells.
- **HTML** — static structure: mode toggle → scoreboard → status line → 3×3 grid of `.cell` divs → reset button. Cells use inline `onclick="play(i)"` with index 0–8.
- **JS** (`<script>`) — pure vanilla, no framework. Key globals: `board` (9-element array), `current` ('X'|'O'), `gameOver`, `scores`, `mode` ('2p'|'ai').

### JS call flow

```
play(i)        — entry point for human clicks; blocks CPU turn in ai mode
  mark(i)      — writes to board[], updates DOM, checks win/draw
    getWinner  — checks all 8 WINS combos; returns { winner, line } or null
    isDraw     — all cells filled
cpuMove()      — called via setTimeout(400ms) after human move in ai mode
  getBestMove  — greedy: win > block > preferred order [center, corners, edges]
```

`resetBoard()` clears state and DOM for a new round; scores persist across resets.
`setMode()` switches between '2p' and 'ai', relabels the O score box, calls resetBoard.
