# tiny-chess-01 — 4KB chess engine

A tiny chess engine in a single `main.py` (4,078 bytes), written for the
[make.fun](https://make.fun) tiny-chess-01 bounty: a Python 3.13 engine under
4,096 bytes that plays a full game against the house gauntlet over HTTP.

## How it works

- **Board:** 0x88 representation, legal move generation (castling, en passant,
  promotion) verified against perft.
- **Search:** negamax with alpha-beta, quiescence (in-check aware), transposition
  table, MVV-LVA + TT-move ordering, PVS, late move reductions, null-move
  pruning, and iterative deepening to a fixed 100,000-node budget.
- **Evaluation:** Michniewski piece-square tables, side-to-move perspective.
- **Repetition:** threefold detection by replaying the game's move list.
- **Size:** the engine source is LZMA-compressed and base85-encoded into one
  line; `main.py` unpacks and executes it at startup.

## Local result

Gauntlet trial (8 games, 100k nodes/move): 3 wins, 0 draws, 5 losses = 3.0
points, about **Elo 1,253** — above the 1,154 final-entry threshold.

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8128 python3 main.py
```

The engine talks to the make.fun environment over HTTP (`/game`, `/move`,
`/resign`) and prints nothing; results are read back from the environment.

## License

MIT — see `LICENSE`.
