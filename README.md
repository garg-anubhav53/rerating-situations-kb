# Re-Rating Situations KB

A durable, machine-built library of **historical stock re-ratings** — companies whose valuation
**multiplied (≥2x) within a single calendar year** — and, for the notable ones, a clear write-up of
**what the market misunderstood, what changed in the business vs. what changed in the multiple, and why
they re-rated so fast.**

The point isn't to chase these after the fact. It's to **study great investment situations** and build
pattern-recognition for spotting the *next* one.

## How it's built
An autonomous agent (a scheduled Claude Code routine, every 3 hours) studies **one year per run**,
walking backward from 2025. Each run:
1. Scouts the year broadly for every ≥2x mover (sonnet, by bucket).
2. Dedups + classifies by magnitude (haiku).
3. Writes them up — **depth scales with magnitude**: 2–3x gets a couple of sentences; 5–10x and 10x+
   (especially surprising small-caps) get a full story (opus).
4. Updates the index + cursor and commits back here.

See **`ROUTINE.md`** for the full operating playbook and **`STATE.md`** for the year cursor.

## Layout
```
ROUTINE.md        # the per-run operating playbook (model tiering, depth rules, git flow)
STATE.md          # the YEAR CURSOR — which year runs next
kb/INDEX.md       # master table of every captured name (all ≥2x)
kb/SEEN.md        # dedup guard (TICKER|YEAR keys already captured)
years/<YYYY>.md   # the write-ups for each studied year
runs/<UTC>.md     # per-run logs
```

## Reading the depth
- **2–3x** — a couple of sentences: what changed.
- **3–5x** — a paragraph or two: what changed + what the market had misjudged.
- **5–10x / 10x+** — the full story: the misunderstanding, business-change vs. re-rating-change, the
  catalyst timeline, and the transferable lesson. The surprising small-caps get the most ink.
