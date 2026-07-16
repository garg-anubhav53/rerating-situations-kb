# STATE — Re-Rating Situations KB

The single source of truth for **which year each run studies**. One year is fully captured per run.

## YEAR CURSOR
```
NEXT_YEAR: 2023          # the year the next run will study
DIRECTION: backward      # after a year is done, NEXT_YEAR = NEXT_YEAR - 1
FLOOR_YEAR: 2000         # do not walk below this; revisit/deepen earlier years instead
PARTIAL:                 # (cleared) 2024 fully captured — all notable ≥3–5x rows prose?=Y
YEARS_DONE: [2025, 2024] # append each fully-captured year, e.g. [2025, 2024]
```

## COUNTERS
```
COMPANIES_CAPTURED: 112
LAST_RUN_UTC:      2026-07-16T12:45:44Z
LAST_RUN_SUMMARY:  Year 2024 PARTIAL→DONE. Resume run: no fresh scouting; enriched the 11 remaining notable 3–5x rows to prose?=Y — 8 sonnet write-ups [VKTX,ALAB,YPF,TLN,SLS,HUMA,BTCS,FTEL] + 3 rows resolved via the GGAL Milei-bank cluster addendum [BMA,BBAR,SUPV]. Roster complete AND every notable ≥3–5x row prose?=Y → 2024 DONE. Cursor 2024→2023, YEARS_DONE=[2025,2024], PARTIAL cleared. 22 2–3x-tail rows remain roster-only by design (non-blocking). Thin buckets (Japan/Korea/China-HK, India micro, US OTC/biotech microcaps) could get a future dedicated 2024 microcap sweep. Next run studies 2023. See runs/2026-07-16T12-45-44Z.md.
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
