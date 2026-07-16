# STATE — Re-Rating Situations KB

The single source of truth for **which year each run studies**. One year is fully captured per run.

## YEAR CURSOR
```
NEXT_YEAR: 2022          # the year the next run will study
DIRECTION: backward      # after a year is done, NEXT_YEAR = NEXT_YEAR - 1
FLOOR_YEAR: 2000         # do not walk below this; revisit/deepen earlier years instead
PARTIAL:                 # (cleared) 2023 fully captured — all notable ≥3–5x rows prose?=Y
YEARS_DONE: [2025, 2024, 2023] # append each fully-captured year, e.g. [2025, 2024]
```

## COUNTERS
```
COMPANIES_CAPTURED: 222
LAST_RUN_UTC:      2026-07-16T19:46:42Z
LAST_RUN_SUMMARY:  Year 2022 study IN PROGRESS — §4a roster PUSHED EARLY (32 ≥2x names). See runs/2026-07-16T19-46-42Z.md. [prior] Year 2023 fresh study → DONE in one run. 5 sonnet scouts → 78 ≥2x names captured (tiers: 2 10x+, 9 5-10x, 32 3-5x, 35 2-3x). Roster+INDEX+SEEN (78 keys) PUSHED EARLY before prose. Then enriched ALL 43 notable (≥3-5x) rows to prose?=Y across waves: 8 top-opus deep-dives (CVNA 10-11x, ALAR 10x+ nano, Ecopro world-#1, VFS 9x de-SPAC squeeze, BTBT, MIGI, AMAM, PRAX) + 3 opus (NVDA, BTC-miner cluster [MARA/CIFR/RIOT/CLSK/WULF/BITF/APLD], India PSU/rail/defense cluster [RVNL/IRFC/SUZLON/MAZDOCK/GRSE/TITAGARH/FACT/JWL/IRCON]) + 11 sonnet (MSTR,COIN,SMCI,MDGL,META,UEC,EOSE,LUCD + Korea-battery/Asia-AI-semi/biotech-catalyst clusters). Two engines: AI breakout (NVDA +239%) + BTC +156% miners. China/HK NEGATIVE (no confident ≥2x). 35 2-3x-tail rows remain roster-only by design (non-blocking). Roster complete AND every notable ≥3-5x prose?=Y → 2023 DONE. Cursor 2023→2022, YEARS_DONE=[2025,2024,2023], PARTIAL cleared. Next run studies 2022. See runs/2026-07-16T15-46-07Z.md.
```

## CURSOR RULES (see ROUTINE.md §5)
- Each run studies exactly `TARGET_YEAR` (= `PARTIAL` if set, else `NEXT_YEAR`).
- Year fully covered → append to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR - 1`, clear `PARTIAL`.
- Year hit the search budget with ≥2x names still unwritten → leave `NEXT_YEAR`, set `PARTIAL: TARGET_YEAR`.
