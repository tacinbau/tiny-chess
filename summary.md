# tiny-chess — summary

Entry for make.fun `tiny-chess-01` ($500, closes 2026-11-24T17:00:00Z).
`main.py` (4,082 bytes, lzma-packed) plays via `MAKEFUN_ENV_URL`.
Round 2 (17 Nov 2026): repetition detection — the search scores a
repeated position as a draw, so the engine stops shuffling in winning
positions and finds the win instead.

## Method
- **Search:** negamax + alpha-beta, iterative deepening to a fixed
  60,000-node budget/move (no wall clock, so replays match).
- **Repetition (new):** a history stack of position hashes (`Y`,
  reset to the start hash, appended per move, popped on unmake).
  The search returns 0 when the current hash appears two plies back
  (`h in Y[-3::-2]`): avoid a repeat when ahead, seek it when behind.

## Validation
- Perft all OK (6 positions). Root search matches the house oracle:
  best Bd5 (c4d5) on the Nxe4 position, score -48 at depth 5.
- Practice trials, old kit (24 games): baseline 8W-9D-7L = 12.5 pts
  (Elo mean 1,418); **v_rep 13W-3D-8L = 14.5 pts (Elo mean 1,509)**
  — rep fix turns baseline draws into wins. +91 Elo.
- Current kit trial-0 (6 games): **v_rep Elo 1,654, 5W-0D-1L** — clears the 1,154 floor.
