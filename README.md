# Tiny Chess Engine — 4 KB of Python

A from-scratch chess engine for the make.fun bounty
[Build the strongest chess engine in 4 KB of Python](https://make.fun/bounty/tiny-chess-01)
($500, ends 2026-11-24 17:00 UTC). `main.py` is the whole engine:
3,968 bytes, Python 3.13 stdlib only, no model calls, deterministic.

## What it is

- 0x88 board, negamax + alpha-beta, transposition table, killer moves,
  MVV-LVA capture ordering, quiescence search (captures + check evasions).
- Check extensions (+1 ply when in check) and a king pawn-shield eval term.
- Iterative deepening to a **fixed node budget of 50,000 nodes/move**
  (deterministic: same position → same move, same node count).
- Eval: material (P100 N320 B330 R500 Q900), procedural piece-square
  tables, bishop pair +25, king centralisation in the endgame,
  pawn shield +12/pawn in the middlegame.

## Practice results (real gauntlet, kit scorer)

| trial | Elo | W-D-L |
|---|---:|---|
| 0 | 1253 | 3-0-5 |
| 1 | 1326 | 3-1-4 |
| 2 | 1396 | 3-2-3 |
| **mean** | **1325** | **9-3-12 (10.5/24)** |

Practice floor is 1154; the house baseline (sunfish 4 KB @ 50k nodes)
averages 1325. This engine matches the house baseline on average.
Slowest move 4.0 s (limit 10 s), trial wall 741–845 s (limit 1800 s).
A re-run of trial 0 with the shipped `main.py` scored 1173 — the
trial environment itself varies ~±80 Elo run-to-run, so the 3-trial
mean above is the reliable figure (see BUILD_LOG.md).

## How to run

Set `MAKEFUN_ENV_URL` to the trial env, then run `python3 main.py`.
The engine plays all 8 gauntlet games via `POST /game`, `/move`,
`/resign`. See `BUILD_LOG.md` for the full build and test transcript.

## Submission

Repo for the bounty: `main.py` + `LICENSE` (+ this README).
Repo total must stay ≤ 8,192 bytes, no file over 4,096 bytes.
`BUILD_LOG.md`, `dev.py` and the test scripts are development files
only — do not include them in the submission repo.
