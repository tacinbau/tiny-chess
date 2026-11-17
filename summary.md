# tiny-chess — summary

Entry for make.fun `tiny-chess-01` ($500, closes 2026-11-24T17:00:00Z).
`main.py` (4,095 bytes, lzma-packed) plays via `MAKEFUN_ENV_URL`.
Round 3 (17 Nov 2026): endgame node budget — 120k nodes once total
non-pawn material drops below 2000, where the nodes-500k anchor wins
long endgames. The eval is honest there; the fix is search depth.

## Method
- **Search:** negamax + alpha-beta, iterative deepening, fixed 60,000
  nodes/move (no wall clock, so replays match).
- **Repetition:** position-hash history (`Y`); search returns 0 when
  the current hash appears two plies back (`h in Y[-3::-2]`).
- **Endgame budget (new):** `J=NB+NB*(Q<2000)` in th() — 120k nodes
  in true endgames, 60k elsewhere. Endgame branching is low, so the
  extra depth fits the 10 s/move limit.

## Validation
- Perft all OK (6 positions). Root: best Bd5 (c4d5), ev root -119.
- Round 2 (V-REP): 24 old-kit games, Elo mean 1,509 (+91).
- Round 3 (V-REP-EB): 18 new-kit games, **14W-3D-1L = 15.5 pts, Elo
  mean 1,706** vs V-REP 14.0 pts / 1,581 — **+124 Elo, +1.5 pts**.
  Won/tied every trial (t0 tie, t1 +1.0, t2 +0.5); t1 game 6
  (nodes-500k) flipped loss to win. Max think 9,964 ms, no timeout
  in 18 games.
