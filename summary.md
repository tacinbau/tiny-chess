# tiny-chess — summary

Entry for make.fun `tiny-chess-01` ($500, closes 2026-11-24T17:00:00Z).
`main.py` (4,090 bytes, lzma-packed) plays the 8-game gauntlet
through the env at `MAKEFUN_ENV_URL`.

## Method
- **Board:** 0x88, pieces `type | colour<<3`, moves `from|to<<7|promo<<14`.
- **Search:** negamax + alpha-beta, iterative deepening to a fixed
  60,000-node budget/move (no wall clock, so replays match). TT
  (Zobrist), killers, MVV-LVA, late-move reduction, quiescence
  (captures, promotions, check evasions).
- **Eval:** incremental material + formula piece-square tables
  (pawn advance/centre, centralisation, rook 7th rank, tapered king).
  Static eval matches the house evaluator exactly on the test
  sequence (-119, +115, -127, +135, -509, +505).
- **Driver:** sets up from `start_fen`, replays the game's moves,
  thinks, posts the UCI move. Deterministic: moves derive from the
  position, the game's moves and the trial seed only.

## Size
- `main.py` 4,090 bytes (cap 4,096). Repo total under 8,192.
- No file over 4,096 bytes.

## Validation
- Perft all OK: startpos d4 197281, kiwipete d4 4085603, pos3 d5
  674624, pos4 d4 422333, pos5 d4 2103487, pos6 d4 3894594.
- Root search matches the house oracle: best Bd5 (c4d5) on the
  Nxe4 position, same score (-48 at depth 5).
- Think ~2-3 s/move at 60,000 nodes (~25-30k NPS); full trial
  fits the 1,800 s wall.
- Practice trial-0: **Elo 1,464** (3 won, 3 drawn, 2 lost). Beat
  depth-2/3/4 (checkmate); drew nodes-100k/250k/1M (threefold);
  lost to nodes-500k/2M. Mean move 2.5 s, max 5.7 s (limit 10 s).
  Beats the sunfish baseline mean (~1,325) by 139; clears the
  1,154 floor to enter the final.
