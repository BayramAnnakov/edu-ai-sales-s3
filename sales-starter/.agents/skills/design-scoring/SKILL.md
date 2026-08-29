---
name: design-scoring
description: Build a lead scoring model (Fit + Intent + Timing) from what this repo already knows about who bought, marking every weight as observed, borrowed or guessed. Writes signals/qualification/scoring-model.md with the tiers, their distinct next actions, the disqualifiers, and the questions the model cannot answer from outside. Use when the user says /design-scoring, wants to score or qualify leads, asks "how should we prioritise inbound", or has no scoring model yet.
---

# Design Scoring

Build the model that turns a lead into a number, and the number into a next action.

> Companion to `qualify-lead`, which applies the model this skill writes. Run this one first.
> If `signals/qualification/scoring-model.md` already exists, read it and **revise** it — do not
> start over. A model that gets rebuilt from scratch every quarter has no memory and cannot learn.

**The rule this skill exists to enforce:** a weight is a claim about who buys. Most scoring models
are a table of numbers somebody made up in an afternoon, and they are never revisited because
nobody remembers which numbers were measured and which were invented. **This skill makes every
weight declare where it came from.**

---

## Step 0 · Read what this repo already knows

Before asking the user anything, read — in this order — and say what you found:

| source | what it gives you |
|---|---|
| `CLAUDE.md` §1 | the ICP and the **"We never touch"** list. The second one is your disqualifiers, already written. |
| `CLAUDE.md` §2 | the autonomy baseline — tells you which arrow you are scoring for |
| `customers/*/` | **who actually bought, and who did not.** This is the only source of an observed weight. |
| `signals/targeting/` | what a previous lead-search run could and could not source |
| `signals/qualification/` | anything already learned about who gets let through |
| `leads/*.csv` | funnel counts, prior scored lists |

If §1 is still a placeholder, **stop.** A scoring model built without an ICP is a table of numbers
about nobody. Ask for two lines: who you sell to, who you decline.

⚠️ **Do not read the leads you are about to score.** Inbound messages and outbound replies waiting
in `leads/` are the thing this model will be *tested* on. Build the model from the contract and the
outcome record, then let `qualify-lead` meet the leads for the first time.

This is not fussiness. A model built while looking at the fourteen messages it will score will fit
those fourteen, describe them back to you in its own criteria, and feel accurate — because you tuned
it until it was. You will have learned nothing about the next fourteen, and you will have destroyed
the only honest test you had. **If you have already read them, say so in the output** rather than
pretending otherwise; a model that declares it was tuned on its test set is merely weak, and one
that hides it is misleading.

**Say out loud how many closed outcomes you found.** Two won deals is not a pattern. Report the
number before you use it, so the user can judge the weights you are about to propose:

```
OUTCOMES AVAILABLE
  closed won     N
  closed lost    M   (of which reason recorded: K)
  open           …
  => weights derived from outcomes:  [which ones]
  => weights borrowed or guessed:    [which ones]
```

---

## Step 1 · Disqualifiers first, and they are not scores

A disqualifier is a **contract decision**, not a low number. Run it before scoring, and never let
a strong Intent score outvote it.

Take the **"We never touch"** line from `CLAUDE.md` §1 verbatim, then add the categories below that
apply to any B2B seller.

🔴 **PRINT THE LIST, MARK EVERY ADDITION, AND KEEP GOING — do not stop and wait for approval.**
Anything not taken verbatim from `CLAUDE.md` §1 is your proposal, not their contract, so label it
`⚠️ NOT IN THE CONTRACT` and carry on. The user reviews the finished model; a skill that blocks on a
question halfway through produces nothing at all when it is run unattended, which is how it is
usually run. **Never silently promote a proposal into the contract** — the label is what makes
finishing without approval safe. Offer at the end to write the confirmed ones into `CLAUDE.md` §1.

- **a competitor** researching you — the single most expensive miss, because they score *high* on
  every intent signal you have. They ask precise questions, they ask them fast, and they want
  documentation.
- **not the buying entity** — an agency, consultant, reseller or analyst asking on behalf of a
  client they have not named. **You cannot score a company you cannot see.**
- **outside the served geography or legal footprint**
- **structurally too small or too large** — below the price floor, or an enterprise that will buy
  a different product from someone else
- **an existing customer** arriving through the wrong door

⚠️ **Every disqualifier needs a stated test**, not just a name. "A competitor" is not testable.
*"Email domain matches a known competitor, or the enquiry seeks specification rather than outcome
and never states a problem of its own"* is testable — and it is the one that catches the polite
ones, who do not use an obvious domain.

⚠️ **A disqualification is still an answer.** Write what the decline says and what it recommends
instead. A business too small for you this year may not be too small in three, and the people you
turned away honestly are the ones who come back.

---

## Step 2 · Fit, Intent, Timing — and where each one actually comes from

The three parts are not interchangeable, and the difference is **which of them you can observe
before a conversation.**

| | what it asks | where it comes from | how much you can see from outside |
|---|---|---|---|
| **Fit** (0-40) | Are they the kind of company that buys from us? | your own won/lost record | **most of it** — size, industry, geography, stack are observable |
| **Intent** (0-40) | Do they want to solve this, now, with someone? | the message and the behaviour around it | **some of it** — and much less on an outbound reply than on an inbound form |
| **Timing** (0-20) | Is there a reason this moves this quarter? | trigger events | **the trigger, yes. The budget, almost never.** |

For each criterion you propose, print the weight **and its provenance**:

```
CRITERION            [what you are measuring]
  WEIGHT             [points, and out of what]
  PROVENANCE         OBSERVED  — derived from N closed outcomes in this repo
                     BORROWED  — a benchmark or an industry rule; name its source
                     GUESSED   — nobody knows; this is a starting hypothesis
  OBSERVABLE FROM    the message · the company's public record · only a call
  IF WRONG           what happens to a lead this criterion misjudges
```

**Three rules on weights, each of which exists because its opposite is the common failure:**

1. **Do not invent precision.** If you have eleven won deals, you cannot justify the difference
   between +7 and +8. Use bands you can defend — `strong / acceptable / weak / no` — and map them
   to numbers afterwards. A model that outputs 73 when the evidence supports "probably good" is
   lying about how much it knows.
2. **A criterion that scores everyone the same does no work.** If every candidate is in the United
   States because you only sell in the United States, geography is not a criterion, it is a
   disqualifier you already applied. Delete it from the score. *(This is the DELETE verdict from
   `find-leads`, and it is as common here as it was there.)*
3. **Never score a criterion you cannot observe.** If budget is unknowable from outside — and it
   usually is — it must not carry points. Zero points for "no budget signal" silently punishes
   every lead who simply did not mention money, which is most of them.

---

## Step 3 · What the model cannot see becomes the first call

This is where the criteria that `find-leads` gave the verdict **CALL** arrive. They did not
disappear. They stopped being targeting criteria and became **questions**.

For each one, write the question as it would actually be asked out loud:

```
UNKNOWABLE           budget
  WHY                nobody publishes what they have to spend
  PROXY, IF ANY      contract renewal date · headcount band · whether they name a current vendor
                     (each of these is evidence about capacity, not about intent to spend)
  ASK                "What did you spend on the tool you're replacing?"
  WHERE IT LANDS     first call, question 2
```

⚠️ **The proxy and the answer are different objects.** A renewal date tells you money exists on a
schedule; it does not tell you it is available to you. Keep them separate in the model, or the
first confident-sounding proxy will quietly become the answer.

Collect these into a **first-call question list** in the output. That list, not the score, is what
a salesperson actually uses on Monday.

---

## Step 4 · Tiers must have different next actions

Propose tiers, then apply the test that kills most tier schemes:

> **If two tiers get the same next action, they are one tier.**

For each tier, name: the action, who does it, and by when. If you cannot fill in all three, the
tier is decoration.

⚠️ **Start with four — Hot, Warm, Cold, DQ — and earn the fifth.** The test above cuts tiers that
duplicate each other; it does not stop you inventing new ones, and the failure mode of this step is
a beautifully reasoned seven-tier scheme that no sales team will ever hold in their head on a
Tuesday. Add a fifth tier only when you can name an action that **none of the four can express**, and say plainly
what that action is and who owns it. ⚠️ Do not go looking for a fifth — you will find one. Add it
only when a real lead in front of you cannot be served by any of the four. A model with more tiers than the team has behaviours is a taxonomy, not a
process.

```
TIER      SCORE/100  NEXT ACTION                     OWNER        WITHIN
HOT       …          a specific action, not "engage" a role       a real interval
WARM      …
COLD      …
DQ        —          the decline, and what it offers instead
```

🔴 **Thresholds are expressed out of 100, never in raw points.** `qualify-lead` drops criteria it
could not observe out of the denominator, so two leads can be scored against different totals. A
threshold in raw points silently punishes the lead with more unknowns — which is the exact failure
the UNKNOWN rule exists to prevent, reintroduced one step later.

⚠️ **Response time is a promise, not an aspiration.** Set an interval the team can actually hold
with the people it has, then let the tier decide who gets the fast one. A one-hour SLA that is met
20% of the time is worse than a four-hour SLA that is met — it teaches the team the number is
fiction.

---

## Step 5 · Write the model → `signals/qualification/scoring-model.md`

```markdown
# Scoring model — YYYY-MM-DD

**Built from:** N closed-won · M closed-lost · [what else]
**Scores which arrow:** inbound / outbound replies / both
**Revision of:** [previous file, or "first version"]

## Disqualifiers — run before scoring
| Rule | Test | What the decline says |

## Fit (0-40) · Intent (0-40) · Timing (0-20)
| Criterion | Weight | Provenance | Observable from | If wrong |

## Tiers
| Tier | Score | Next action | Owner | Within |

## What this model cannot see
| Unknown | Proxy, if any | The question | Where it is asked |

## What would prove this model wrong
[see Step 6]

## Weights I guessed
[list them, plainly. This is the section a reader should look at first.]
```

## Step 5b · Write the questions as their own file — this is a required output

🔴 **Write `signals/qualification/first-call-questions.md`. Named exactly that. Every run.**

The model file is read by agents; this one is read by a person, on a call, on a Monday. It is the
only artifact of the two that anybody carries into a conversation, and it is the reason the
"CALL" verdicts from lead-search stop being a shrug and become a script.

**Measured 28 Aug: without this step stated as a filename, a clean run skipped the file entirely** —
it folded the questions into the model and moved on. "Where a human will find them" is not an
instruction a machine can check itself against. A filename is.

```markdown
# First call — the questions

**Last updated:** YYYY-MM-DD
**Why this page exists:** everything below is something no database will tell you.

### 1 · "<the question, as you would actually say it>"
*What it settles:* <which criterion>
*Why you have to ask:* <what the sourcing run found — how often it was resolvable>
*Ask it this way,* not <the version that gets a useless answer>
```

One heading per unknown from Step 3. If there are none, write the file anyway and say so — a model
with nothing to ask about is a claim worth seeing in writing.

---

## Step 6 · State what would falsify it

A scoring model is a set of predictions about who will buy. Predictions can be wrong, and a model
nobody can prove wrong is not a model.

Write down, now, before any lead is scored:

🔴 **Name ONE outcome and ONE window, before anything else in this block.** "Did it work" is not a
question a row can answer unless every row is predicting the same event by the same date. Write it
literally, and make it the definition of `outcome` in every CSV this model produces:

```
PREDICTED EVENT   [e.g. a first meeting actually held — not booked, held]
WINDOW            [e.g. within 30 days of predicted_on]
RECORDED BY       a human, in the `outcome` column
```

Pick the earliest event you can observe honestly. "Bought" is the one that matters and it is
useless here — at a 45-day cycle you learn nothing until the quarter is over, so the loop never
closes and the model never improves. A held meeting is observable in days.

```
THIS MODEL PREDICTS
  HOT leads reach that event at a materially higher rate than WARM.
  The disqualifiers exclude nobody who would have bought.

IT IS WRONG IF
  · HOT and WARM convert at the same rate      -> the weights carry no information
  · a DQ'd lead buys from a competitor          -> a disqualifier is too wide
  · every lead scores 55-70                     -> the model does not discriminate; see below
  · the highest scores are all leads who wrote long messages
                                                -> you scored verbosity, not intent

CHECK IT AFTER  [N] scored leads, or [date] — whichever comes first
```

⚠️ **The clustering failure is the common one and it is invisible without this check.** A model
where every lead lands in a narrow band feels like it is working — it produces numbers, the numbers
differ slightly, the tiers get assigned. It is doing nothing. Say what range you expect, so the
failure is detectable.

---

## Step 7 · Report

- the file you wrote
- **how many weights are GUESSED** — this is the headline, not the model
- the tiers, and the one action that distinguishes each
- the first-call questions, as a list
- what you would need in order to replace the guesses with observations, and roughly when the
  repo would have it

Never present a guessed weight in the same voice as a measured one. A model that sounds equally
confident about all of its numbers has destroyed the only information the user needed.
