# STATE — Re-Rating Situations KB

The single source of truth for **which year each run studies**. One year is fully captured per run.

## YEAR CURSOR
```
NEXT_YEAR: 2022          # the year the next run will study
DIRECTION: backward      # after a year is done, NEXT_YEAR = NEXT_YEAR - 1
FLOOR_YEAR: 2000         # do not walk below this; revisit/deepen earlier years instead
PARTIAL: 2022            # 2022 notable rows all prose?=Y, BUT B2 micro/nano + biotech thin → micro deep-sweep pending
YEARS_DONE: [2025, 2024, 2023] # append each fully-captured year, e.g. [2025, 2024]
```

## COUNTERS
```
COMPANIES_CAPTURED: 220
LAST_RUN_UTC:      2026-07-16T19:46:42Z
LAST_RUN_SUMMARY:  Year 2022 studied (rare BEAR-market year: S&P -19%, Nasdaq -33%; ≥2x leaderboard = one macro trade — energy/commodity supply shock post-Ukraine + defense re-armament + a few squeezes). 5 sonnet scouts → 32 ≥2x names captured & roster PUSHED EARLY before prose. Enriched all 9 notable (≥3–5x) rows to prose?=Y: opus deep-dives INDO (10x+ nano oil panic-spike, round-tripped), SGML (5–6x Brazil lithium), CEIX (coal cash-machine, ~3x/4xTR); sonnet STNG/TNK (tankers on Russia rerouting), HNRG (micro coal), ADANIPOWER (Adani/coal shortage), THYAO (BIST lira melt-up), JSW.WA (Polish coking coal). Only 2 S&P 500 doublers all year: OXY (+122%) & CEG (+109%). VERIFICATION REMOVALS: REPX (~5x was 2021→2022 arc, cal-2022 only +60%) and ENSV (spike was Feb-2021 GameStop squeeze, fell in 2022) — both dropped from roster/INDEX/SEEN as not ≥2x-within-2022. Final 30 names (1 10x+, 1 5–10x, 6 3–5x, 22 2–3x). Cursor NOT advanced — B2 micro/nano + biotech buckets thin (no structured screener) → PARTIAL: 2022 for a dedicated micro-cap deep-sweep next run (skips 30 SEEN names). See runs/2026-07-16T19-46-42Z.md. 5 sonnet scouts → 78 ≥2x names captured (tiers: 2 10x+, 9 5-10x, 32 3-5x, 35 2-3x). Roster+INDEX+SEEN (78 keys) PUSHED EARLY before prose. Then enriched ALL 43 notable (≥3-5x) rows to prose?=Y across waves: 8 top-opus deep-dives (CVNA 10-11x, ALAR 10x+ nano, Ecopro world-#1, VFS 9x de-SPAC squeeze, BTBT, MIGI, AMAM, PRAX) + 3 opus (NVDA, BTC-miner cluster [MARA/CIFR/RIOT/CLSK/WULF/BITF/APLD], India PSU/rail/defense cluster [RVNL/IRFC/SUZLON/MAZDOCK/GRSE/TITAGARH/FACT/JWL/IRCON]) + 11 sonnet (MSTR,COIN,SMCI,MDGL,META,UEC,EOSE,LUCD + Korea-battery/Asia-AI-semi/biotech-catalyst clusters). Two engines: AI breakout (NVDA +239%) + BTC +156% miners. China/HK NEGATIVE (no confident ≥2x). 35 2-3x-tail rows remain roster-only by design (non-blocking). Roster complete AND every notable ≥3-5x prose?=Y → 2023 DONE. Cursor 2023→2022, YEARS_DONE=[2025,2024,2023], PARTIAL cleared. Next run studies 2022. See runs/2026-07-16T15-46-07Z.md.
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
