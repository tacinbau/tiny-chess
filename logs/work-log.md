# tiny-chess work log

## 2026-11-13
- Kit fetched; RULES.txt + kit.json read. Entry `main.py` <=4,096 B.
- Built 0x88 engine: negamax + alpha-beta, TT, killers, MVV-LVA,
  LMR, quiescence, incremental material + formula PST.
- Perft all OK (6 positions). Fixed eval bug (make() ignored piece
  colour); ev() matches the house evaluator; root best Bd5.
- Golfed `main.py` to 7,453 B readable; packed to 4,090 B.
  Dropped: bishop-pair, repetition detection.

## 2026-11-14
- Fixed driver sync bug: set up from `start_fen` + replay moves
  (was double-applying from `fen`). Trial-0 re-run.

## 2026-11-17 (round 2)
- Restored repetition detection (`Y` hash history). v_rep: 24 games,
  13W-3D-8L = 14.5 pts, Elo mean 1,509 vs baseline 1,418 (+91).
  New kit trial-0: Elo 1,654, 5W-0D-1L. Packed to 4,082 B.
- Dropped variants: PVS -68, node budget 80k -138, bishop-pair -291,
  check extension ~0.

## Results
- Practice trial-0 (packed, NB 60000): **Elo 1,464** (3W-3D-2L).
  W vs depth-2/3/4; D vs nodes-100k/250k/1M; L vs nodes-500k/2M.
  Mean move 2.5 s, max 5.7 s (limit 10 s). Beats the sunfish
  baseline mean (~1,325) by 139; clears the 1,154 final floor.
