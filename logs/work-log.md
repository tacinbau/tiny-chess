# tiny-chess work log

## 2026-11-13
- Kit fetched; RULES.txt + kit.json read. Entry `main.py` <=4,096 B.
- Built 0x88 engine (`dev.py`): negamax + alpha-beta, TT, killers,
  MVV-LVA, LMR, quiescence, incremental material + formula PST.
- Perft all OK (startpos d4 197281, kiwipete d4 4085603, pos3 d5
  674624, pos4 d4 422333, pos5 d4 2103487, pos6 d4 3894594).
- Fixed eval bug: make() ignored piece colour in the incremental
  score. Verified ev() matches the house evaluator; root best Bd5.
- LMR-fixed drew vs depth-2 (Italian Game, 3-fold repetition).
- Golfed `main.py` (8,122 -> 7,453 B readable); lzma+base85 packed
  to 4,090 B (cap 4,096). Dropped: bishop-pair, repetition detection.

## 2026-11-14
- Fixed driver sync bug: set up from `start_fen` + replay moves
  (was double-applying from `fen`). Trial-0 re-run.

## Results
- Practice trial-0 (packed, NB 60000): **Elo 1,464** (3W-3D-2L).
  W vs depth-2/3/4; D vs nodes-100k/250k/1M; L vs nodes-500k/2M.
  Mean move 2.5 s, max 5.7 s (limit 10 s). Beats the sunfish
  baseline mean (~1,325) by 139; clears the 1,154 final floor.
