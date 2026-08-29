---
name: qualify-lead
description: Score inbound leads and outbound replies against the model in signals/qualification/scoring-model.md, draft the tier-appropriate reply, and record the score as a prediction that a human can later mark right or wrong. Writes leads/qualified-YYYY-MM-DD.csv. Use when the user says /qualify-lead, hands over an inbound message, a batch of leads, or a set of replies to cold outreach.
---

# Qualify Lead

Turn a message into a tier, a next action, and a draft — and write the score down **as a
prediction**, so that someone can find out later whether it was right.

> Companion to `design-scoring`, which writes the model this skill applies.
> No model yet? Say so and run `design-scoring` first. Do not invent one silently — a scoring
> model the user never saw is a set of opinions with a number attached.

**The rule this skill exists to enforce:** a score is a prediction about what this lead will do.
Nobody at ServiceGrid ever checked one: an LLM classifier labelled 500 replies `interested`, 145
became opportunities, and no human ever compared the label to the outcome. **So this skill records
what it predicted, and forbids itself from recording what happened.**

---

## Step 0 · Read the repo before you read the lead

In this order, and say what you found:

1. `signals/qualification/scoring-model.md` — the model. If absent, stop and say so.
2. `CLAUDE.md` §1 — the ICP and the "We never touch" list.
3. **`customers/` — every folder.** Before scoring anyone, check whether this person or this
   company is already in there.
4. `signals/` — anything a previous run learned.

⚠️ **Step 0.3 is not a formality and it is the one that gets skipped.** A returning lead is not a
new lead. If the company has a folder, that folder may already contain the reason the last deal
died, the dealbreaker nobody answered, and how many calls it took. A scorer that treats a
returning prospect as a fresh inbound will re-run the conversation that already failed, and it will
score them *higher* than a stranger — because they arrive knowing what to ask for.

If you find a match, say so **before the score**:

```
⚠️ ALREADY IN THIS REPO
  company        [name] -> customers/[folder]/
  last outcome   [status, date]
  what killed it [from the file, quoted]
  unanswered     [the question nobody answered last time]
```

---

## Step 1 · Disqualify before you score

Run the disqualifier list from the model. **A disqualifier is not a low score, it is a different
verdict**, and it must not be reachable by adding points.

The three that need active checking rather than pattern-matching:

- **Competitor.** Not just a domain match — most are not that careless. The tell is the *shape* of
  the enquiry: it seeks specification rather than outcome, and it never states a problem of its own.
  **A buyer describes a pain. A competitor requests a spec.** ⚠️ This has a real false positive —
  some technical buyers genuinely do ask implementation questions first. The tell is not that they
  asked; it is that they asked and never said what hurts.
- **Not the buying entity.** A consultant, agency or advisor running a selection on behalf of an
  unnamed client. You cannot score a company you have not been told the name of. This is not a
  refusal to engage — it is a refusal to *score*. Ask who the client is; until then, `Fit` is
  unknown and must be recorded as unknown, not as zero.
- **Contract exclusions.** Whatever `CLAUDE.md` §1 says under "We never touch" — apply it even when
  the lead is enthusiastic, well-written and ready to buy. **That is exactly when it is hard**, and
  a contract you break for enthusiastic leads is not a contract.

For every disqualification, record **which rule fired** and draft the decline anyway. See Step 5.

---

## Step 2 · Research only what changes the score

For each lead, resolve what the model actually needs — company size band, industry, geography,
stack, trigger events. Use whatever is connected (a company-data MCP, web search, the company's own
site), and **name what you used**.

🔴 **HARD RESEARCH BUDGET: at most three lookups per lead, and none at all for a lead already
disqualified.** When the budget is spent, whatever is still missing is **UNKNOWN** — write it in
`unknowns` and move on. UNKNOWN is a legitimate, informative result; a fourth search is almost never
the one that changes the tier.

⚠️ **This cap is the difference between a skill that finishes and a skill that hangs.** Measured on
this sample case: fourteen leads, one run finished the scored CSV in under ten minutes; a second run
with no cap was still searching after four hours and had written nothing at all. An agent researching
an unfindable fact does not fail — it keeps looking, silently, forever. **Budget first, then look.**

Announce the budget before you start, and report what you spent:

```
RESEARCH PLAN
  leads to score        N     (of which disqualified on read: K -> 0 lookups)
  lookup budget         <= 3 x (N - K)
  stopping rule         budget spent, or the tier can no longer move
```

Stop early whenever further research cannot move the tier. Research is not free and most of it does
not change the answer.

**Three things to look for that people miss:**

- **the trigger buried in a fact.** A named departure, a stated renewal month, an audit date, a
  lease ending — these are deadlines wearing the clothes of small talk. The trigger is rarely in the
  adjectives ("urgently", "excited"); it is in the nouns and the dates.
- **the dealbreaker stated as a question.** A specific, early question about an integration, a
  compliance rule or a data-residency requirement is not curiosity — it is a pass/fail condition,
  and it is the single most common reason a good-fit deal dies quietly. Record it in `condition`,
  never as an intent signal: enthusiasm about a product that cannot meet the condition is not
  intent, it is a loss you have not had yet.
- **what is missing.** A message with no company name, no size, no timeline is not a low-intent
  message. It is an **unknown**, and it must be scored as one.

---

## Step 3 · Score, and separate the three kinds of number

Apply the model. For every criterion, one of exactly three states:

| state | meaning | how it scores |
|---|---|---|
| **VERIFIED** | you resolved it from a source you can name | full weight, up or down |
| **ASSUMED** | you inferred it, plausibly, from something adjacent | full weight, **and it is listed in `assumed`** |
| **UNKNOWN** | you could not see it at all | **no points either way**, and it is listed in `unknowns` |

⚠️ **UNKNOWN is not zero.** Scoring an unseen criterion as zero punishes leads for what they did
not happen to mention, and it is why almost every homegrown model ranks talkative leads above
serious ones. If Fit has four criteria and you could only see two, say the Fit score is out of 20,
not out of 40, and say so in the row.

Report the score as a **band with the arithmetic shown**, never as a bare number:

```
LEAD        [name] · [company]
FIT         [n]/[out of]   verified: … | assumed: … | unknown: …
INTENT      [n]/[out of]
TIMING      [n]/[out of]
-----------------------------------------------
SCORE       [score]/[out of]  =  [score_100]/100   <- the tier is read from THIS
TIER        [tier]         confidence: high / medium / low
WHY         one sentence a salesperson would repeat out loud
```

**Confidence is separate from score, and both are needed.** A 70 with three unknowns and a 70 with
everything verified call for different next actions. A model that collapses them has thrown away
the distinction that decides whether to research or to phone.

---

## Step 4 · Write the list → `leads/qualified-YYYY-MM-DD.csv`

```
name,company,source,tier,score,score_out_of,score_100,confidence,fit,intent,timing,verified,assumed,unknowns,dq_rule,trigger,condition,first_call_question,would_change_tier,next_action,owner,due,predicted_on,outcome,outcome_recorded_by
```

- **`source`** — `inbound_form` · `outbound_reply` · `referral` · … The same score means different
  things on an inbound form and on a reply to a cold message; if you cannot tell them apart later,
  you cannot learn from either.
- **`score_out_of`** — the denominator you actually scored against, after removing UNKNOWN
  criteria. A bare score with a hidden denominator is the whole problem this column solves.
  ⚠️ **It must equal the sum of the three per-block denominators, and you must check that it does.**
  Write `fit`, `intent` and `timing` as `n/out_of` and add the three right-hand sides. This
  arithmetic has already been got wrong once on this exact sample — a row scored `26/30 · 8/40 ·
  0/8` and reported `score_out_of` as 68 instead of 78 — and a wrong denominator silently moves a
  lead's percentage, which is what the tier is read from. **If your three blocks do not sum to your
  total, the row is wrong: fix it before writing, do not round it away.**
- **`score_100`** — `round(100 × score ÷ score_out_of)`. **This, and only this, is what the tier is
  read from**, and it is the 0-100 number the model is described in.

🔴 **THE STEP THAT MAKES "UNKNOWN IS NOT ZERO" ACTUALLY TRUE.** Removing an unseen criterion from
the denominator accomplishes nothing if the tier is then read off the raw numerator — the lead is
simply penalised later instead of sooner. A lead scoring 45 of a possible 84 is at **54**, not 45,
and against a HOT threshold of 60 that is the difference between a phone call and a nurture email.
**Compare `score_100` to the thresholds. Never `score`.** If your model's thresholds were written
against raw points, convert them once, in the model file, and say you did.
- **`unknowns`** — semicolon-separated. These are the first-call questions for this specific lead.
- **`condition`** — a stated pass/fail requirement (an integration, a compliance rule, a payroll
  format). **Not an intent signal.** A condition that fails kills a HOT lead.
- **`would_change_tier`** — the one fact that would move this lead up or down a tier. This is the
  most useful column in the file: it is the research task, written by the thing that noticed it was
  missing.
- **`next_action` / `owner` / `due`** — from the model's tier table. If any is blank, the tier did
  no work.
- **`predicted_on`** — today. The score is a prediction made on a date.
- **`outcome`** — **always write empty.** It means: what this lead actually did — meeting, no
  reply, bought, went elsewhere. **You may never fill this in**, in this run or any later one, and
  neither may any other agent. Only a human who knows what happened may write here.
- **`outcome_recorded_by`** — the name of that human. Empty otherwise.

Append if the file exists. Never overwrite.

⚠️ **Why `outcome` is locked, in one line:** the model predicted the tier, so the model cannot be
the thing that grades the prediction. A system that scores its own predictions correct will
converge on being confident, not on being right. *(Same rule, same reason, as `evidence_checked` in
`find-leads`.)*

---

## Step 5 · Draft the replies → `leads/replies-drafted-YYYY-MM-DD.md`

One draft per lead, **including the disqualified ones.**

Each draft is shaped by its tier's next action, and each opens on the specific thing this person
said. The rules from `find-leads` hold here and one is added:

1. **Open on their stated problem, in their words.** Not on your product, not on a compliment.
2. **Answer the condition, if they stated one.** Someone who asks "does it integrate with
   named system and receives a paragraph about your platform's flexibility has been told the answer
   is no. If you do not know, say you will find out, and give a date.
3. **Ask the highest-value unknown — one, not five.** The first-call question list is for the call,
   not the email. Pick the one that would move the tier.
4. **Never state a tier or a score to the lead.** Obvious, and it has shipped.
5. **A decline is a real message.** Say no clearly, say why in one line, and point somewhere useful.
   Do not write a warm non-answer that leaves them following up for three weeks.

⚠️ **Draft only. Never send, never schedule, never open a mail client.**

---

## Step 6 · File what you learned → `signals/qualification/`

Patterns across the batch belong to the **model**, not to the leads. Write
`signals/qualification/qualification-YYYY-MM-DD.md`:

```markdown
# Qualification run — YYYY-MM-DD

**Scored:** N leads · [breakdown by source]
**Model:** [file, date]

| Tier | N | Notes |

## What the model could not see
| Criterion | Unknown for | Why | Verdict: proxy · call · delete |

## Where the model looks wrong already
[before any outcome is known, some failures are visible: a tier with no leads in it, every
lead in one band, a disqualifier that fired on nobody, a criterion that scored identically
for everyone]

## Predictions to check
[which leads, and by when, and what result would mean the model is wrong]
```

Then **say which line of the model to change**, and offer to edit it. A signal that never changes
the model is a diary entry.

---

## Step 7 · Report

- the files you wrote, with counts by tier
- **the leads whose tier you are least confident about, and what you would need to resolve them** —
  this is the headline, not the hot list
- 🔴 **every lead that no tier actually fits.** Say so out loud rather than filing them under the
  nearest one. Ask of every lead: *is there an action this tier's action would get wrong?* If yes,
  no tier fits, and the lead will land in whatever it falls through to while the model looks like it
  worked.
  **A missing tier is the most common defect in a first scoring model, and the run is the only
  moment it is visible** — afterwards these become quiet rows nobody re-reads. Name them, say which
  action they actually needed, and offer to add the tier.
- any lead already in `customers/`, and what the old file says
- any stated condition that would kill a high-scoring lead
- the one lead to contact first, the action, and by when

Never report a tier without its confidence, and never report a score without saying what it was out
of. The number is not the deliverable — **the next action, and the honest account of what the
number does not know, are the deliverable.**

---

## Step 8 · When a human overrides you, write down why

🔴 **This is the only step that makes the next run better than this one.** Everything above produces
a prediction. This step is what turns a corrected prediction into a changed model instead of a
one-off edit that nobody remembers.

Whenever the user moves a lead's tier, rejects a draft, or tells you a disqualifier fired wrongly,
**ask for the reason in one line and record it** — before you touch anything else:

```markdown
## <date> · <lead> · <what I said> -> <what it should be>
**Reason (their words):** …
**Which rule was wrong:** a weight · a threshold · a disqualifier · a missing tier · nothing —
                          the model was right and the data was wrong
**Change proposed:** the specific line in scoring-model.md, or "none — one-off"
```

Append it to `signals/qualification/corrections-YYYY-MM.md`, then say which line of
`signals/qualification/scoring-model.md` you would change and offer to make the edit.

**Three rules, and the third is the one that keeps this honest:**

1. **One correction is an anecdote.** Do not rewrite a weight because a single lead was misjudged.
   Record it, and propose the change when the same rule has been corrected two or three times.
2. **Distinguish "the model was wrong" from "the data was wrong."** A lead misjudged because a fact
   was unfindable is an enrichment problem, not a scoring problem, and reweighting will not fix it —
   it will just move the error somewhere less visible.
3. ⚠️ **A correction is not an outcome.** The human saying "this should be HOT" is a better-informed
   opinion, not evidence the lead converts. It goes in `corrections-*`. The `outcome` column still
   only ever records **what the lead actually did**, and it is still yours to leave alone.

*This mirrors what teams running this in production do: when a draft is corrected, the reason goes
back into the skill so the same mistake is not made twice. The correction is the product.*
