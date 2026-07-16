# STATE — Re-Rating Situations KB

The single source of truth for **which year each run studies**. One year is fully captured per run.

## YEAR CURSOR
```
NEXT_YEAR: 2024          # the year the next run will study
DIRECTION: backward      # after a year is done, NEXT_YEAR = NEXT_YEAR - 1
FLOOR_YEAR: 2000         # do not walk below this; revisit/deepen earlier years instead
PARTIAL:                 # (cleared) 2025 fully captured; next run starts fresh on 2024
YEARS_DONE: [2025]       # append each fully-captured year, e.g. [2025, 2024]
```

## COUNTERS
```
COMPANIES_CAPTURED: 61
LAST_RUN_UTC:      2026-07-16T06:46:42Z
LAST_RUN_SUMMARY:  Year 2025 COMPLETE. Resume pass enriched the final 6 notable 3–5x rows to prose?=Y (298040.KS Hyosung Heavy, 267260.KS HD Hyundai Electric, TLSA, PALV, RR, BLSH) via sonnet writers (6/8 cap). All tier ≥3–5x rows now have prose. Roster complete → advanced cursor: YEARS_DONE=[2025], NEXT_YEAR=2024, PARTIAL cleared. 2–3x tier (28 names) left as roster one-liners (best-effort, not required for finalize).
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
