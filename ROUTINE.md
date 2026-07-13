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

## 4. STEP 4 — WRITE (early comprehensive push, THEN capped enrichment waves)

**#1 ROBUSTNESS RULE — persist the roster BEFORE the expensive prose, and push after every wave.** A
run has a *bounded work budget*, not a wall-clock timeout. The old "do all writes, push once at the end"
shape burns the whole budget on opus prose and gets cut off before the single push → the run commits
NOTHING. Never again: push cheap-and-early, then enrich incrementally so a killed run always leaves
progress behind.

### 4a. ROSTER + EARLY PUSH (haiku) — do this FIRST, before any opus/sonnet prose
One haiku agent (`claude-haiku-4-5-20251001`) writes the full capture at minimal depth, then you **PUSH
IMMEDIATELY (§6)**:
- Append EVERY deduped ≥2x name to **`kb/INDEX.md`** (one row each — this IS the comprehensive capture).
- Create/append **`years/<TARGET_YEAR>.md`** with a **roster table** at the top:
  `ticker | exch | multiple | tier | surprising? | start mkt-cap | one-line what-changed | prose?`
  — set **`prose?` = N** for every row.
- Append `TICKER|YEAR` keys to **`kb/SEEN.md`**. Write **`runs/<UTC>.md`** (started; scout coverage; tier counts).
- Update **`STATE.md`** counters / `LAST_RUN_*`.
- **PUSH NOW.** The full ≥2x roster is now safe even if everything after this dies.

### 4b. ENRICHMENT WAVES (depth by tier; PUSH after each wave of ~4)
Now enrich with prose, **highest-value first**, appending a prose section under the roster in
`years/<TARGET_YEAR>.md` and flipping that row's `prose?` to **Y**.

**COMMIT IN WAVES AFTER ANY INTERESTING FINDING — do not batch.** The moment an interesting write-up
lands, persist it:
- **Every opus-tier finding (10x+, 5–10x, surprising) → its OWN immediate commit + push (§6)** as soon
  as that single write-up is appended. Never hold a big/surprising story in memory waiting for others.
- Lighter tiers (3–5x, 2–3x) → push after each small wave of ~4.
So a run that dies is never more than one write-up away from being fully saved, and the repo shows
progress continuously.

Order + HARD per-run caps (hit a cap → stop, leave the rest `prose?=N` for a later run):

| Order | Tier | Writer model | Depth | Cap / run |
|---|---|---|---|---|
| 1 | **10x+**, and **SURPRISING 5–10x** | `claude-opus-4-8` | **Full write-up — go long.** This is the point of the KB. | ≤ 8 |
| 2 | other **5–10x** | `claude-opus-4-8` | Full write-up (see template). | ≤ 6 |
| 3 | **3–5x** | `claude-sonnet-4-6` | 1–2 tight paragraphs. | ≤ 8 |
| 4 | **2–3x** | `claude-haiku-4-5-20251001` | A couple of sentences (only if budget remains — the roster one-liner already captures it). | best-effort |

**§2 doctrine — heavy writers get NO schema.** They WRITE PROSE (append to the year file) and return a
one-line done/failed status. Cap deep writers ≤8–10 searches, short writers ≤3, then stop-and-write.
Fan out ~4–6 concurrent; drop null/slow results (`.filter(Boolean)`); never block on a straggler.

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

## 5. STEP 5 — FINALIZE CURSOR (haiku)

The INDEX / SEEN / `runs/` writes already happened in §4a. This step only advances the cursor. One haiku
agent updates **`STATE.md`** and pushes (§6):
- Bump `COMPANIES_CAPTURED`; refresh `LAST_RUN_UTC` / `LAST_RUN_SUMMARY`.
- **Advance ONLY if the year is fully done** = roster complete AND every *notable* row (tier ≥ 3–5x) in
  `years/<TARGET_YEAR>.md` has `prose?=Y`:
  - **Done** → append `TARGET_YEAR` to `YEARS_DONE`, set `NEXT_YEAR = TARGET_YEAR − 1`, clear `PARTIAL`.
  - **Not done** (a cap was hit and notable rows are still `prose?=N`, or scouts were budget-capped with
    more ≥2x names to find) → set **`PARTIAL: TARGET_YEAR`** and DO NOT advance.

**How a `PARTIAL` year resumes (no wasted re-work):** when `TARGET_YEAR` came from `PARTIAL`, SKIP fresh
scouting of names already in `kb/SEEN.md`; instead read `years/<TARGET_YEAR>.md`, take the notable rows
with `prose?=N`, and run §4b enrichment on those (flipping them to `Y`). Optionally run ONE cheap sonnet
scout to catch ≥2x names the first pass missed, appending new roster rows (`prose?=N`) + INDEX + SEEN.
The year advances once no notable `prose?=N` rows remain.

---

## 6. STEP 6 — COMMIT + PUSH (reusable; CALL IT REPEATEDLY within a run)

This is not a once-at-the-end step. **Call it after §4a (roster), after EACH opus finding, after each
light wave, and at §5 (cursor).** `origin` already carries the token from the clone. Use an
increment-specific commit message, e.g. `year 2025: roster (47 names)`, `year 2025: +IONQ 10x+ deep-dive`,
`year 2025: 3–5x wave (4)`, `year 2025: cursor → 2024`.

```bash
git add -A
git diff --cached --quiet && echo NO_CHANGES || \
  git -c user.name=rerating-bot -c user.email='garg.anubhav.xyz@gmail.com' \
      commit -q -m "<increment: year <TARGET_YEAR> — what this commit adds>"
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
