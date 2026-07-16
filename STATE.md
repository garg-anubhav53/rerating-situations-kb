# STATE — Re-Rating Situations KB

The single source of truth for **which year each run studies**. One year is fully captured per run.

## YEAR CURSOR
```
NEXT_YEAR: 2023          # the year the next run will study
DIRECTION: backward      # after a year is done, NEXT_YEAR = NEXT_YEAR - 1
FLOOR_YEAR: 2000         # do not walk below this; revisit/deepen earlier years instead
PARTIAL: 2023            # roster complete (78 names); enrichment of notable ≥3–5x rows in progress
YEARS_DONE: [2025, 2024] # append each fully-captured year, e.g. [2025, 2024]
```

## COUNTERS
```
COMPANIES_CAPTURED: 190
LAST_RUN_UTC:      2026-07-16T15:46:07Z
LAST_RUN_SUMMARY:  Year 2023 fresh study. 5 sonnet scouts → 78 ≥2x names captured (tiers: 2x 10x+, 9x 5-10x, 30x 3-5x, 37x 2-3x). Roster + INDEX + SEEN (78 keys) + run log written and PUSHED EARLY before prose. Two engines defined the year: AI breakout (NVDA +239% epicenter, META/AMD/AVGO, SMCI, Taiwan/Japan semis) and BTC +156% recovery leveraging miners 3-10x (MARA +688%, RIOT, CLSK, CIFR, BTBT, WULF, MIGI). Regional clusters: Korea battery mania (Ecopro +571% = world #1 stock), India PSU/rail/defense (RVNL, IRFC, Suzlon, Mazagon Dock, etc.), Argentina Milei trade (GGAL/YPF/PAM). China/HK came back NEGATIVE (not thin — no confident ≥2x). Enrichment waves in progress this run; PARTIAL:2023 set (41 notable ≥3–5x rows, enrichment-capped). Next run resumes 2023 PARTIAL. See runs/2026-07-16T15-46-07Z.md.
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
