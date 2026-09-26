---
name: review-pipeline
description: Review a sales pipeline from a CRM export or a CRM connected over MCP - velocity with its inputs stated, which deals are dormant, what each rep's numbers can and cannot say, and what closed deals say about who buys. Writes pipeline.md (derived, never hand-edited), dashboard.html and routed signals. Refuses to compute until the CRM's definitions are written down. Use when the user says /review-pipeline, brings a CRM export or deal list, asks "who is my best rep", "what is our sales velocity", "which deals are stuck/dormant/zombies", "what is our real conversion", "reverse-engineer our ICP from closed deals", or asks for a pipeline dashboard.
---

# Review Pipeline

A CRM does not record what happened. It records **what somebody clicked**, and when. Every rate
and every duration in it is measured from a click whose meaning nobody wrote down: when a deal is
created, what "won" means, whether a dead deal ever gets closed.

This skill computes the pipeline numbers **after** writing those meanings down, checks each number
three ways before believing it, and writes down what it could not tell.

> Session 1's `audit-funnel` reads funnel totals. This skill reads the deals underneath them.
> `debrief-calls` reads one deal from inside. This one reads all of them from above — and hands
> back the questions only the calls can answer.

**The rule this skill exists to enforce:** *define the clock before you compute.* A number whose
definition you did not state is not a finding. It is a default somebody else chose.

---

## Step 0 · Definitions first. No arithmetic before this step is done.

1. **Find the data.** In this order: what the user gave you; a CRM connected over MCP (say which
   server and which tools you called); any CSV export in `leads/`. If there is a README next to the
   export, read it — it is the process owner talking. Several candidates → name them and ask.
2. **Read the Definitions section of `CLAUDE.md`** (a heading containing "Definitions"). It should
   answer four questions:

   | question | why it moves the numbers |
   |---|---|
   | What makes somebody click "create opportunity"? | Too early inflates the denominator and stretches the cycle. Too late (deals entered after they succeed) pushes conversion toward 100% and shrinks the cycle to nothing. |
   | What does "won" mean here? | Signed contract? Meeting booked with the decision-maker? A team that sells in two stages has two meanings of won. |
   | When does the cycle start, and when does it stop? | Created → closed, first meeting → signature — they are different numbers, and both are legitimate. Pick one and say which. |
   | When is an open deal considered dormant? | "No activity for more than N days before `as_of`." Without it, every open deal counts as pipeline forever. |

3. **If any answer is missing or blank:** look at the data for evidence, **show the rows that are
   your evidence**, propose an answer, and **ask the user to confirm or correct it.** Do not compute
   until the four answers exist. If the user wants to proceed without answering, write the proposed
   answer into your output labelled `[ASSUMED]` and carry that label on every number that depends
   on it.
4. **State `as_of`.** The date the export was taken, not today. Every "days since" is computed
   against it.
5. **Name what each column can and cannot mean.** A field that is only filled after a meeting
   (incumbent, budget, decision-maker) is blank for everyone who never met you — a blank there is
   an artefact of the process, not a property of the customer. Say which fields are blank by
   construction **before** anything is compared across them.

---

## Step 1 · Compute — by code, never in your head

⚠️ Write and run a script. Percentages done in your head are exactly the class of error this skill
exists to catch in others.

**Decided means closed.** A win rate is wins ÷ deals that are **closed (won or lost)** in the
population you name. Open deals go in neither the numerator nor the denominator — and say how many
open ones you left out, because they will resolve one way or the other and move the rate. Report
the range: what the rate would be if every open deal lost, and if every open deal won.

Compute, and label every line with its definition:

1. **The funnel inside the CRM** — from created, through whatever stage evidence exists (a first
   meeting, a proposal), to won. Each step on decided deals.
2. **Velocity**

   ```
   velocity ($/day) = N open deals × W win rate × A average won deal ÷ L cycle length (days)
   ```

   It is a **stock divided by a time**: the deals in the pipe now, draining at the rate they
   historically close. Say out loud what each letter is in this CRM. Then compute it **twice**:

   - **as the CRM stands** — every open deal counts, the definitions as they come;
   - **cleaned** — dormant deals out of N, W on decided deals from your confirmed definitions.

   Then the **sanity check**: compare both with the won value per day the export actually shows over
   the period it covers — and say whether that value is a signed amount or a list price. A velocity several times anything the company has ever booked is a
   statement about the records, not about the future. ⚠️ This is a sanity check, not a forecast
   and not a back-test — say so. Neither version is "the true forecast".

   Also show this identity, because it surprises people: if N is *every deal of the period* and the
   rate is wins ÷ *every deal of the period* (not the decided-only W above), then N × rate is simply
   the number of wins, whatever "opportunity" means — and only L is left to move. Say which rate you
   used; with the decided-only W the identity does not hold.
3. **Per rep — the four levers separately, never only the product.** For each rep: N, W, A, L,
   with **n next to every rate**. Then, for each lever, who **controls** it and who only
   **influences** it:
   - N — usually the router, the territory, or the manager who sets them;
   - A — usually pricing and segment;
   - W and L — influenced by the rep, and also by deal mix, product fit and the buyer.
   Rank the reps under **more than one definition** of velocity and show where the ranking
   changes. If it changes, that is the finding: the answer to "who is best" depends on a choice
   somebody has to make on purpose.

---

## Step 2 · Three checks — written into the output, not done silently

Every number you are about to put in `pipeline.md` goes through all three. Write the result.

1. **Sanity** — against a benchmark **and against a base rate**. For any rate on a small group,
   give n, the rate it is being compared with, and how surprising the difference would be by
   chance (an exact test or an interval — say which and which direction). Use words that match the
   evidence: "insufficient evidence to tell these apart" is not "the same"; a p-value just under
   0.05 after many comparisons is not a finding. **Say how many cuts you looked at.** If you
   scanned twenty thresholds and report the best one, say twenty.
2. **Cross-validate** — against a second source that was produced independently: the funnel
   totals in `leads/`, a `customers/<company>/TIMELINE.md`, a call transcript, the user's memory.
   Totals that disagree are a finding; say which you trust and why.
3. **Spot-check** — for every bucket you name ("dormant", "never met", "lost on price"), open
   `min(5, size)` random rows and say what they actually were. **Name the bucket from what you saw
   in the rows, not from the filter you used to build it.**

---

## Step 3 · Dormant deals — review candidates, not dead ones

Using the dormant definition from Step 0 and `as_of`:

- list the dormant open deals with their amount and stage, and **sum them** against the period's
  actual bookings;
- **cross-tab stage × activity.** A deal whose stage says one thing and whose activity says another
  (a meeting happened, the stage never moved; "Proposal" and silent for months) is a record-keeping
  finding in its own right;
- give each one a **next action**: `review` (the default), re-engage with a named question, or
  escalate. **Never propose closing deals in bulk.** Closing one is a decision for the person who owns
  it, after they have looked; write "close?" only as a question for them, one deal at a time. A
  stop-loss is a **review trigger**, not a closing rule — a number of calls alone does
  not mean a deal is dead; calls *without progress* do. Check whether any **won** deals needed as
  many calls before you propose a threshold.

⚠️ Dormant is not lost. Buyers come back. Present "zero value" for dormant deals as a scenario,
not as a correction.

---

## Step 3b · Record-keeping lint — what is wrong with the records, and how to stop it recurring

Every CRM is a mess in the same few ways. Run each check below **by code**, give the count and up to
five example ids, and for every defect you find name **one prevention** from the three layers.

| check | what it catches |
|---|---|
| a meeting or call is recorded, but the stage never moved | the stage is typed by hand and was forgotten |
| the first meeting happened **before** the deal was created, or a won deal was created within a few days of closing | a deal entered after the fact (for a bonus, at month end). It shortens the measured cycle. It inflates the win rate **only if** the deals that were lost were never entered at all — the dates alone do not show that; say so |
| two records for the same company (normalise the name: case, punctuation, "Co."/"Company"/"Inc.", "&"/"and"; compare state and size too) — and whether they have different owners, or one of them is already a customer | **possible** duplicates: two reps chasing one buyer, or prospecting your own customer. A matching name is a candidate, not proof — the owner confirms |
| a closed-lost deal with no loss reason | a required field that was not required |
| a meeting is recorded but no call, note or email is logged against it | activity that happened outside the CRM. **If the export has no event log (only counts), write `NOT ASSESSABLE — no activity events` rather than a count** |
| open deals silent past the dormant threshold (Step 3) | nobody closes the dead |

**Prevention, in three layers — name the one that fits each defect:**
1. **At the click** — make the right action the only possible one: a required field, a duplicate check
   on create, the definition of "create a deal" written where the person clicking will see it.
2. **Every week** — this lint, run on a schedule, with each finding sent to the owner of the record.
   A defect found weekly costs a minute; found at quarter end it costs the forecast.
3. **Repair from evidence** — when a call transcript, a calendar entry or an email shows what really
   happened, propose the fix to the record **with that evidence quoted**.

**If the CRM is connected over MCP and has write tools:** propose each repair as one line — record,
current value, proposed value, **the definition it satisfies** (a stage change needs a written stage
definition — if there is none, propose an owner review instead), evidence — and **apply it only after
the user confirms that one repair.** Never batch-apply. After applying, add a note to the record saying what changed, why, and
the evidence. Never close, merge or delete a record yourself: propose it to the owner.

## Step 4 · Who buys — reverse-engineering the ICP from closed deals

Compare **won against lost among decided deals that reached a real conversation.** Deals that never
met you say nothing about fit; they say something about the step before.

For every attribute you compare (size band, incumbent, region, source…):
- a table with **won / decided and n in every cell**;
- the fields that are **blank by construction** (Step 0.5) are excluded, and you say so;
- **with a few dozen wins, most patterns are noise.** Say how many attributes and cuts you looked
  at, and label every row with exactly one of:
  - `NOISE AT THIS n` — the default. Any single comparison whose p is above ~0.01 after you have
    looked at several cuts belongs here, however striking the zero looks.
  - `HYPOTHESIS` — still weak on the numbers, **but** a second, independent source points the same
    way (a call transcript, a customer file, a signal already in `signals/`). Name that source.
  - `SIGNAL` — survives the sanity check **with the number of comparisons taken into account**, and
    you can say why it would be true. Rare at this sample size.
  An honest table that is mostly `NOISE` is a good result.

Then say what the CRM **cannot** tell you about fit, and **who could**: the customers who already
bought. **List every won customer by name and id** under the heading `## Who could tell us` in
`signals/unrouted/pipeline-YYYY-MM-DD.md` — they are the cheapest test of any fit hypothesis, and of
any open question in `signals/` that nobody could answer from the CRM.

---

## Step 5 · Write the files

**`pipeline.md`** at the repo root — derived, never edited by hand. Header:

```
# Pipeline — as of YYYY-MM-DD
> Derived by review-pipeline from <source>. Do not edit by hand: re-run the skill.
> Definitions used: <the four answers, with [ASSUMED] where the user did not confirm>
```

Then, in this order: velocity (as-is, cleaned, sanity check) · the funnel inside the CRM · per-rep
levers with n · dormant deals with next actions · **record-keeping lint (check · count · ids · prevention
layer)** · who buys (with SIGNAL / NOISE labels) · what could
not be told. **Tag every number `MEASURED` or `INFERRED`.**

**`dashboard.html`** at the repo root — one self-contained file: no CDN, no external fonts, no
network calls, opens from disk with wifi off. Inline SVG or plain HTML tables. It shows the same
numbers as `pipeline.md` and **the definitions at the top**. A chart without its definitions is
the thing this skill exists to prevent.

**Signals, routed** — follow `CLAUDE.md` §"signals/ is not a diary":
- `signals/qualification/pipeline-YYYY-MM-DD.md` — anything about **what counts** as an
  opportunity, a stage, a win;
- `signals/targeting/pipeline-YYYY-MM-DD.md` — a fit finding labelled `SIGNAL` only. Never `NOISE`,
  never `HYPOTHESIS`: a hypothesis goes to `signals/unrouted/` with the test that would settle it;
- `signals/unrouted/pipeline-YYYY-MM-DD.md` — questions whose owner is not sales (product, pricing,
  the router).

**Required: check every earlier file in `signals/` against your numbers.** Grep them for counts
and labels about the pipeline — totals, "won", "lost", "open", "opportunities", any number you also
computed. Where one states something your numbers contradict (for example, it counts something as
finished that the export shows as still in progress), **append a section to the end of that file**:

```
## Correction — YYYY-MM-DD (review-pipeline)
The text above is kept as written. <file> said: "<exact quote>". The export shows: <what, with n>.
Source: <export file or MCP pull, as_of>.
```

Never edit the original text. If you checked and found nothing to correct, say so in the report.

**`CLAUDE.md` → `## Definitions`** — if the user confirmed the four answers in Step 0, write them
there, dated. That is the one hand-maintained thing this skill produces.

---

## Step 6 · Report on screen

Short enough to read on a projector, in this order:

1. **The definitions** — four lines, with `[ASSUMED]` where applicable.
2. **Velocity: as the CRM stands / cleaned / actual bookings per day.** One line each.
3. **The biggest gap between what the CRM says and what the rows say** — one sentence and the row
   ids you opened.
4. **Per rep:** the lever that differs most, with n — and whether the ranking survives a change of
   definition.
5. **Dormant:** count and value against the period's bookings.
5b. **Record-keeping:** one line per lint check that found something — count and the prevention layer.
6. **Files written** — including every correction appended to an earlier `signals/` file (or "checked
   N files, none contradicted"), and the path of the `## Who could tell us` list.

---

## What you are forbidden to write

- ⛔ **A rate without its n**, or a comparison without the population it is computed on.
- ⛔ **"Best rep" / "worst rep"** as a conclusion. Describe levers and behaviour, not people. Never
  name a real salesperson as a poor performer in a file that gets shared.
- ⛔ **A forecast.** You can compute velocity and sanity-check it; you cannot promise it.
- ⛔ **A cause read off a field that was filled by hand afterwards** (a loss reason, a stage) without
  saying it was filled by hand afterwards.
- ⛔ **A pattern from a blank-by-construction field.**
- ⛔ **Anything you did not compute.** If a number came from the user or from another file, say so.

## Cost control

- Do not research companies on the web. Everything you need is in the export and the repo.
- One script, re-run as needed; do not recompute by hand to "double-check".
- A CRM over MCP: pull the deals once into a local file, say how many and when, then work from it.
