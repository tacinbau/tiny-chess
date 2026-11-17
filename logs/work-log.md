# tiny-chess work log

## 2026-11-13
- Kit fetched; RULES.txt + kit.json read. Entry `main.py` <=4,096 B.
- Built 0x88 engine: negamax + alpha-beta, TT, killers, MVV-LVA,
  LMR, quiescence, incremental material+PST.
- Perft all OK. ev() matches the house evaluator.

## 2026-11-17 (round 2)
- Restored repetition detection (`Y` hash history). v_rep: 24 games,
  13W-3D-8L = 14.5 pts, Elo 1,509 vs baseline 1,418 (+91).
  New-kit t0: Elo 1,654, 5W-0D-1L. Packed to 4,082 B.
- Dropped variants: PVS -68, node budget 80k -138, bishop-pair -291,
  check extension ~0.

## 2026-11-17 (round 3)
- Endgame node budget (`J=NB+NB*(Q<2000)`): 120k nodes when Q<2000.
  18 new-kit games: 14W-3D-1L = 15.5 pts, Elo 1,706 vs V-REP
  14.0 pts/1,581 (+124 Elo, +1.5 pts). Packed to 4,095 B.
- Dropped: rep+CE+NMP -375, rep+EB 1.5x -94 (t1 game 6 lost).
