# 🧩 Fifteen Puzzle — Agent Skill for Google AI Edge Gallery

A skill that lets you play the classic 15-puzzle sliding tile game entirely on-device,
powered by Gemma 4 (gemma-4-E2B-it-litert-lm or gemma-4-E4B-it-litert-lm).

## How to Load in Google AI Edge Gallery

1. Host this folder on **GitHub Pages** (see instructions below).
2. In the Gallery app → **Agent Skills** → **+** → **Load from URL**.
3. Enter the URL to your hosted `SKILL.md`, e.g.:
   ```
   https://AvP78.github.io/fifteen-puzzle-skill/SKILL.md
   ```

## How to Play

- **"New game"** — starts a freshly shuffled puzzle.
- **"Up / Down / Left / Right"** — slides tiles into the empty space.
- **"Move tile 5"** — the model figures out the right direction for tile 5.

## File Structure

```
fifteen-puzzle/
├── SKILL.md          ← loaded by Gallery; contains LLM instructions
├── scripts/
│   └── index.html    ← game engine (board generation, move validation)
└── README.md         ← this file
```

## Hosting on GitHub Pages (Recommended)

1. Create a new GitHub repo (e.g. `fifteen-puzzle-skill`).
2. Copy all files into the repo root.
3. Create an empty `.nojekyll` file in the root (prevents Jekyll from processing SKILL.md).
4. Go to **Settings → Pages** → Source: **main branch / root**.
5. Your skill URL will be: `https://AvP78.github.io/fifteen-puzzle-skill/SKILL.md`

> **Important:** GitHub raw URLs (`raw.githubusercontent.com`) won't work —
> use the GitHub Pages deployment URL.

## How It Works

- The **JS engine** (`scripts/index.html`) handles all game logic deterministically:
  - `new_game` → generates a solvable shuffled board (120 random moves from goal state)
  - `move` → validates and applies a move, returns new board + state array
- The **LLM** (`SKILL.md`) interprets player intent, calls `run_js`, and displays results.
- Board state travels in the conversation as a JSON array (`board_state: [...]`),
  so the LLM always has the current position to pass to the next JS call.

## Example Conversation

```
User:   play 15 puzzle!
Gemma:  [calls run_js → new_game]
        Moves: 0

        ┌────┬────┬────┬────┐
        │  5 │  3 │  1 │  7 │
        ├────┼────┼────┼────┤
        │ 12 │    │  2 │  8 │
        ├────┼────┼────┼────┤
        │  9 │ 11 │  4 │ 14 │
        ├────┼────┼────┼────┤
        │ 13 │  6 │ 15 │ 10 │
        └────┴────┴────┴────┘
        Your move! What's next?

User:   To the right
Gemma:  [calls run_js → move, direction:"right"]
        Moves: 1
        ...
```

---

*Skill created for Google AI Edge Gallery · Apache 2.0*
