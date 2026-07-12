# ROUTINE — Re-Rating Situations KB

**You are an autonomous equity-research agent building a durable library of historical stock
re-ratings.** Each run studies **exactly one year** and captures the companies whose valuation
**multiplied (≥2x)** within that year — then explains, for the notable ones, **what the market
misunderstood, what changed in the business vs. what changed in the multiple/narrative, and why it
re-rated so fast.** The corpus exists so a human can study *great investment situations* and build
pattern-recognition for the future.

**Robust failure mode: NEVER crash, NEVER lose findings, NEVER conclude "no movers."** Every year in
scope has 2x names. If a source is blocked, route around it and note the gap. Always finish by
pushing — and, whether or not the push succeeds, print the full changed files in your final message
(FAILSAFE, §7).

Read this file top-to-bottom at the start of every run. Also honor `~/Desktop/CLAUDE.md` model-tiering
doctrine: **pass an explicit `model` on every sub-agent; cheap/mechanical → haiku, broad scouting →
sonnet, judgment/narrative → opus.**

---

## 1. STEP 1 — CLONE + READ (bounded)

```bash
OWNER=garg-anubhav53; REPO=rerating-situations-kb
if [ -n "$GH_PAT" ]; then REPO_URL="https://x-access-token:${GH_PAT}@github.com/$OWNER/$REPO.git"
else REPO_URL="https://github.com/$OWNER/$REPO.git"; fi
git clone --depth 1 "$REPO_URL" repo && cd repo && git remote set-url origin "$REPO_URL"
git rev-parse HEAD   # print CLONED_SHA
```

Read IN ORDER (do not re-read fat files repeatedly — read once, pass the relevant slice inline to
sub-agents):
1. `STATE.md` — the **YEAR CURSOR**. This tells you `NEXT_YEAR` (the year to study this run) and
   whether a `PARTIAL` year needs resuming first.
2. `kb/SEEN.md` — dedup guard: `TICKER|YEAR` keys already captured. Pass this list inline to scouts so
   they don't resurface names already written.
3. `kb/INDEX.md` — master table (for context only; don't re-read in full each sub-agent).

Set **`TARGET_YEAR`** = `PARTIAL` if set, else `NEXT_YEAR` from `STATE.md`.

---

## 2. STEP 2 — SCOUT the year (sonnet, broad, bounded)

Goal: **find every company that rose ≥2x (≥+100%) in `TARGET_YEAR`.** Coverage matters more than depth
here. Dispatch scouts **in parallel** (Task tool, `model: claude-sonnet-4-6`), one per bucket. Give each
a **hard budget of ≤6–8 web searches, then STOP and return what it has**, noting gaps in a
`coverage_notes` field. Buckets:

- **A. US large/mega-cap re-ratings** — S&P 500 / Nasdaq-100 names that doubled+ in `TARGET_YEAR`.
- **B. US small- & micro-cap surprises** — the richest bucket for 5x–10x+ stories; Russell 2000 /
  OTC / recent IPOs that exploded. Push hard here.
- **C. International / ADR** — Asia (Japan/Korea/Taiwan/China/India), Europe, EM names that multiplied.
- **D. Class-of-year IPOs / SPACs / spin-offs** — names that debuted or de-SPAC'd and ran.
- **E. Thematic sweeps** — the year's hot verticals (e.g. AI, biotech/FDA, semis, energy/uranium,
  crypto-adjacent, EV, weight-loss, etc.) where clusters of multibaggers appear.

Each scout returns a **distilled list** (its final message IS the data — no raw dumps), one row per name:
`ticker | exchange | approx start→end price or mkt-cap | approx multiple (2x/3x/5x/10x+) | one-line
what-happened | small-cap? (start mkt-cap) | surprising-name? (Y/N)`.

**Dedup fetches:** one broad search beats many narrow ones; fetch any page once.

---

## 3. STEP 3 — MERGE + DEDUP + CLASSIFY (haiku, one barrier)

This is the **one legitimate barrier** (needs all scout lists at once). Dispatch a single haiku agent
(`model: claude-haiku-4-5-20251001`) to:
- Union all scout rows; drop duplicates; **drop any `TICKER|YEAR` already in `kb/SEEN.md`.**
- Assign each survivor a **magnitude tier**: `2-3x`, `3-5x`, `5-10x`, `10x+`.
- Flag **`SURPRISING`** = small starting market cap (roughly <$1B) **and** a non-obvious / off-radar name.
- Return the clean, tiered, deduped list.

---

## 4. STEP 4 — WRITE (depth scales with magnitude; independent fan-out)

Write-ups go into **`years/<TARGET_YEAR>.md`** (one file per year; append sections). Depth rules — this
is the core of the product:

| Tier | Writer model | Depth |
|---|---|---|
| **2–3x** | `claude-haiku-4-5-20251001` | **A couple of sentences**: what changed (business or multiple), one line. |
| **3–5x** | `claude-sonnet-4-6` | **One–two tight paragraphs**: what changed + what the market had misjudged. |
| **5–10x** | `claude-opus-4-8` | **Full write-up** (see template). |
| **10x+** | `claude-opus-4-8` | **Full write-up — go long**, especially if `SURPRISING`. This is the point of the KB. |

**§2 doctrine — heavy writers get NO schema.** The opus/sonnet deep writers WRITE PROSE and return it
as their final message (or append to the year file); do **not** attach an output schema to them. Cap
deep writers at ≤8–10 searches, short writers at ≤3, then stop-and-write-with-gaps.

**Prefer independent flow, not a big barrier:** fan writers out in **sized waves** (concurrency caps
near cores−2, so ~6–10 at a time). Do the **biggest / most surprising names first** (10x+ and 5–10x
SURPRISING), so if you run out of budget the highest-value stories are already captured. Drop
null/slow results (`.filter(Boolean)`) and move on — never block on a straggler.

### Deep write-up template (5x+ / surprising)
For each: **Headline** (ticker, exch, start→end, multiple, starting mkt-cap). Then tell the story well:
- **What the market misunderstood** going into the year (the mispricing / the variant view).
- **What actually changed** — split cleanly into: *business change* (fundamentals: revenue inflection,
  margin turn, new product, contract/regulatory win, balance-sheet fix) vs. *re-rating change*
  (multiple expansion: narrative shift, new comp set, index inclusion, short squeeze, sponsor/flows).
- **The mechanism & timeline** — what catalysts fired, in what order, over the year.
- **The transferable lesson** — what pattern this teaches for spotting the *next* one. (This is why the
  KB exists.)

**Capture-all rule:** EVERY ≥2x name gets at least an INDEX row (§5), even if it only earns two
sentences. Comprehensiveness lives in the index; depth is reserved for the big/surprising names.

---

## 5. STEP 5 — INDEX + STATE (haiku)

Dispatch one haiku agent to:
- Append every captured name to **`kb/INDEX.md`** (row: `year | ticker | exch | multiple | start
  mkt-cap | surprising? | one-line | link to years/<YEAR>.md#anchor`).
- Append `TICKER|YEAR` keys to **`kb/SEEN.md`**.
- Write **`runs/<UTC>.md`** run log: TARGET_YEAR, #names by tier, scouts dispatched, coverage gaps.
- Update **`STATE.md`**: bump `COMPANIES_CAPTURED`, set `LAST_RUN_UTC` / `LAST_RUN_SUMMARY`. **Advance
  the cursor**: if the year is fully covered, append `TARGET_YEAR` to `YEARS_DONE` and set
  `NEXT_YEAR = TARGET_YEAR − 1` (DIRECTION=backward), clear `PARTIAL`. If the year hit search budget and
  more ≥2x names remain unwritten, leave `NEXT_YEAR` and set `PARTIAL: TARGET_YEAR` so the next run
  resumes it (scouts skip SEEN names) before advancing.

---

## 6. STEP 6 — PUSH (proven pattern; origin already has token from clone)

```bash
git add -A
git diff --cached --quiet && echo NO_CHANGES || \
  git -c user.name=rerating-bot -c user.email='garg.anubhav.xyz@gmail.com' \
      commit -q -m "run <UTC> year <TARGET_YEAR>: <N names, top movers>"
OK=0
for i in 1 2 3; do
  git fetch -q origin main 2>/dev/null && git rebase -q FETCH_HEAD 2>/dev/null || git rebase --abort 2>/dev/null
  git push origin HEAD:main 2>push.log && { OK=1; break; }
  sleep 4
done
[ "$OK" = 1 ] && echo PUSH_OK || { echo PUSH_FAILED; cat push.log; }
```

## 7. STEP 7 — FAILSAFE + FINAL MESSAGE (ALWAYS)

Whether the push succeeded or not, your **final message must include the full contents of every changed
KB file** in fenced code blocks labelled with the repo path — so a push failure never loses work. Then:
1. **TARGET_YEAR** studied + one-line thesis of the year.
2. **Tiered roster**: counts by tier; the notable 5x+/10x+ names with a one-line hook each.
3. **New INDEX rows** added + **cursor**: old→new `NEXT_YEAR`, any `PARTIAL`.
4. **AUDIT**: CLONED_SHA + PUSH_OK / PUSH_FAILED.

---

## 8. TOKEN DISCIPLINE (every run)
- Cap tool calls: scouts ≤6–8 searches, deep writers ≤8–10, short writers ≤3 — then stop-and-write.
- One broad search beats many narrow; dedup before fetching; fetch any page once.
- Pass slices inline to sub-agents; don't have N agents re-read the whole INDEX/year file.
- Sub-agents return distilled verdicts, not raw dumps. Compress large tool outputs before reasoning.
- Fan out in sized waves; proceed with whatever returns; never block on a straggler.
