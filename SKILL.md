---
name: fifteen-puzzle
description: Play the classic 15-puzzle sliding tile game (Пятнашка / Пятнашки). Start a new game or make moves. Triggers on phrases like "сыграем в пятнашки", "fifteen puzzle", "sliding puzzle", "новая игра пятнашки", "move tile", "slide up/down/left/right".
metadata:
  homepage: https://github.com/google-ai-edge/gallery/discussions/categories/skills
---

# 🧩 Fifteen Puzzle — Пятнашка

A classic sliding tile game. Arrange tiles 1–15 in order with the empty space in the bottom-right corner.

## Starting a New Game

When the user wants to play or start a new game, call the `run_js` tool:
- data: `{"action":"new_game"}`

## Making a Move

For every move, call the `run_js` tool:
- data: A JSON string with these fields:
  - `action`: `"move"`
  - `board`: Array of 16 integers — copy EXACTLY from the line `board_state: [...]` in the previous tool result
  - `direction`: Where the empty space [ ] moves. One of: `"up"`, `"down"`, `"left"`, `"right"`
  - `moves`: Integer — copy from `Moves: N` in the previous tool result

## Translating User Input to Direction

The `direction` is where the EMPTY SPACE moves (the tile slides the opposite way):
- User says "вверх" / "up" / "↑" → direction `"up"` (empty moves up, tile below slides up)
- User says "вниз" / "down" / "↓" → direction `"down"`
- User says "влево" / "left" / "←" → direction `"left"`
- User says "вправо" / "right" / "→" → direction `"right"`
- User says "сдвинь плитку N" / "move tile N" → determine which direction empty must move to reach tile N (only works if tile N is adjacent to empty)

## After Each Tool Call

1. Display the board grid from the tool result exactly as returned.
2. Show the move count.
3. If the result contains `won: true` — congratulate the user warmly! 🎉

## Important

- ALWAYS pass the exact `board_state` array from the previous result into the next call.
- If the tool returns an error, explain it to the user and ask them to try a different move.
- After `new_game`, display the board and invite the user to start moving tiles.
