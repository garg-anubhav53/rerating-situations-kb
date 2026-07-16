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
COMPANIES_CAPTURED: 112
LAST_RUN_UTC:      2026-07-16T09:41:02Z
LAST_RUN_SUMMARY:  Year 2024 IN PROGRESS. 6 scouts captured 51 unique ≥2x names (10x+:3, 5–10x:7, 3–5x:19, 2–3x:22). Roster + INDEX + SEEN pushed early (§4a). Enrichment waves (opus 10x+/5–10x/surprising, sonnet 3–5x) follow with per-finding pushes. See runs/2026-07-16T09-41-02Z.md.
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
