# STATE — Re-Rating Situations KB

The single source of truth for **which year each run studies**. One year is fully captured per run.

## YEAR CURSOR
```
NEXT_YEAR: 2025          # the year the next run will study
DIRECTION: backward      # after a year is done, NEXT_YEAR = NEXT_YEAR - 1
FLOOR_YEAR: 2000         # do not walk below this; revisit/deepen earlier years instead
PARTIAL:  2025           # roster captured; enrichment of remaining prose?=N rows continues next run
YEARS_DONE: []           # append each fully-captured year, e.g. [2025, 2024]
```

## COUNTERS
```
COMPANIES_CAPTURED: 61
LAST_RUN_UTC:      2026-07-15T09:09:29Z
LAST_RUN_SUMMARY:  Year 2025 roster captured (61 ≥2x names; 4 at 10x+, 8 at 5–10x). Enrichment in waves; PARTIAL 2025.
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
