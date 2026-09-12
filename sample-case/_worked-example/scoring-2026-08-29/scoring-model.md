# Scoring model — 2026-08-29

**Built from:** 0 closed-won records · 1 closed-lost record (Northline Mechanical) · 1 aggregate
funnel CSV (16 wins, no per-deal rows) · 1 targeting run (`_worked-example/icp-gaps-2026-08-15.md`)
**Scores which arrow:** replies → meetings. Both sources: inbound forms **and** replies to cold outbound.
**Revision of:** first version.

> **Read this line before you read the table.** ServiceGrid has **no per-deal outcome record**.
> 16 companies signed and not one of them is in `customers/`. Everything below is therefore
> **BORROWED or GUESSED**. Exactly **zero** weights in this model are OBSERVED. The model is a set
> of hypotheses about who buys, written down so that it can be proved wrong — it is not a summary
> of who bought.

```
OUTCOMES AVAILABLE
  closed won     0   (the funnel CSV says 16 signed; none has a record. The names are not in this repo.)
  closed lost    1   (Northline Mechanical — reason recorded: yes, and the record is wrong;
                      the file says "went quiet", the call says an unanswered QuickBooks Enterprise
                      dealbreaker on call 1)
  open           0
  => weights derived from outcomes:  none
  => weights borrowed or guessed:    all ten
```

**What was read to build this:** `CLAUDE.md` §1–§3 · `customers/northline-mechanical/CLAUDE.md` ·
`leads/funnel-12mo.csv` · `_worked-example/icp-gaps-2026-08-15.md` and its README ·
`signals/` (all three folders empty).

**What was deliberately NOT read:** `leads/inbound-2026-08-28.md`, `leads/replies-2026-08-28.md`
and their `.ru` twins — the 14 messages this model will be tested on. `leads/inbound-2026-08-01.md`
was opened to its **first four lines only** (date, channel, sender) to establish whether it was an
outcome record or another unscored lead; it is another unscored lead, and its body was not read.

---

## Disqualifiers — run before scoring

A disqualifier is a contract decision. It runs first and a high Intent score can never outvote it.

| Rule | Test | What the decline says |
|---|---|---|
| **National franchise** *(verbatim, `CLAUDE.md` §1)* | Company name appears as a franchisee/brand location of a national network, or the site carries "find a location near you" with >1 state and a national brand. | "You'll get more from an enterprise dispatch suite than from us — we're built for independents running one P&L. If your franchise agreement lets you choose your own field software, come back and we'll talk." |
| **Under 10 technicians** *(verbatim, `CLAUDE.md` §1)* | Published employee count < 15, or one location and one or two named staff, or the message says so. Below the price floor at ~$22k/yr. | "At your size this costs more than it saves. Come back when you pass ~15 in the field — that's usually when dispatch stops fitting in one person's head." |
| ⚠️ **NOT IN THE CONTRACT — Over 200 technicians** | Published tradesperson/employee count > 250, or >8 branches across >3 states. | "You're past what we're built for — you'll want something with multi-entity job costing." Two candidates were already dropped on this in the Aug-15 run (Arden, Murphy), so the ceiling does real work. |
| ⚠️ **NOT IN THE CONTRACT — Outside the US** | Company's operating addresses are outside the US. Implied by "US field-service contractors" in §1 but never written as an exclusion. | "We only operate in the US today." |
| ⚠️ **NOT IN THE CONTRACT — A competitor researching us** | **Shape, not domain.** The enquiry seeks *specification* and never states a problem of its own: asks for feature lists, limits, architecture, pricing tiers, integration matrices — fast, precise, and with no account of what hurts. A buyer describes a pain; a competitor requests a spec. Domain match against known field-service vendors is a second, weaker test — most are not that careless. | Nothing warm and nothing detailed. "Happy to point you at the public pages." ⚠️ **Real false positive:** technical buyers do ask implementation questions first. The tell is not that they asked — it is that they asked **and never said what hurts**. |
| ⚠️ **NOT IN THE CONTRACT — Not the buying entity** | An agency, consultant, reseller, MSP or analyst running a selection for a client they have not named. | **Do not decline and do not score.** See the HOLD row in Tiers — this one is a gap in the tier scheme, not a rejection. |
| ⚠️ **NOT IN THE CONTRACT — Already in `customers/`** | The company or the person matches a folder under `customers/`. | Not a decline. Route it to the AE who owns the folder, and **answer the question that killed it last time before anything else.** Today that is exactly one company: Northline Mechanical, lost 2026-03-28, unanswered question = QuickBooks Enterprise. |

**Offer:** the four ⚠️ rows above are proposals, not ServiceGrid's contract. Say the word and I will
write the confirmed ones into `CLAUDE.md` §1 under "We never touch".

---

## What is NOT scored, and why

Applying "a criterion that scores everyone the same does no work" to the Aug-15 sourcing run:

| Deleted from the score | Why | Where it went instead |
|---|---|---|
| Trade = HVAC / plumbing / mechanical | Sourced 5 of 5. Verdict **delete** in `icp-gaps-2026-08-15.md`. | It defines the search string. |
| US geography | Sourced 5 of 5. Verdict **delete**. | Became a disqualifier above. |
| Not a national franchise | Excluded **zero** candidates. Verdict **delete from the rubric, keep the concern**. | Became a disqualifier above. |
| ACV ~$22,000 fit | Sourced **0 of 5**. No public record exists of a private contractor's software budget. Verdict **call**. | First-call question 1. Carries **no points** — scoring an unknowable criterion silently punishes every lead who did not mention money, which is most of them. |
| Website engagement (pricing-page visits, return visits, content downloads) | `funnel-12mo.csv`: `inbound_volume,0`. There is no inbound motion, no history and no analytics to read. A generic scoring template would put 15 points here. | Nothing. It is not observable, so it does not exist. |

---

## Fit (0-40)

**Bands are the claim; the numbers are a rendering of the bands.** With one lost deal and no won
records, nothing here justifies the difference between 7 and 6. `strong = 10 · acceptable = 7 ·
weak = 3 · no = 0`, chosen so that strong is worth about three times weak.

| Criterion | Weight | Provenance | Observable from | If wrong |
|---|---|---|---|---|
| **F1 · Technician count 20–200** — strong: published field/tech count in band · acceptable: total employee count 25–250 (counts office staff too) · weak: branch count only · UNKNOWN: nothing published | 0–10 | **GUESSED.** The band comes from `CLAUDE.md` §1; nothing in the repo shows a 60-tech shop converting better than a 150-tech one. | Company's own About page (2 of 5 publish a number) · branch count · **job postings — the highest-fidelity free source and never yet run** | A 4-branch residential plumber with 15 techs scores acceptable and is under the price floor. Costs an AE a wasted call. |
| **F2 · Independent, not PE-rolled-up** — strong: named owner-operator, no tracker hit · acceptable: self-described independent, no tracker hit · weak: tracker hit or holding-company language · UNKNOWN: cannot resolve | 0–10 | **BORROWED** from the Aug-15 negative screen against published PE roll-up trackers (Apex, Wrench, Sila, ARS, TurnPoint, Service Experts, Comfort Systems). | The company's own site (**stale after an acquisition — Benoure still reads independent**) + tracker screen | **High precision, low recall.** Everyone the screen flags is genuinely not independent; a quiet local sale or search fund leaves no trace, so we will score acquired shops as strong. |
| **F3 · The writer is the buyer** — strong: owner / co-owner / CFO / president · acceptable: GM or ops director · weak: dispatcher, office manager, tech · no: student, jobseeker | 0–10 | **BORROWED** from `CLAUDE.md` §1 ("buyer is usually the owner, CFO or co-owner") — a stated belief, never checked against the 16 wins. | The signature and the message · the company's leadership page (5 of 5 name a leader — the most reliably sourced thing in this ICP) | An office manager doing real homework for an owner who signs scores weak. The correction is cheap; the miss is not. |
| **F4 · The incumbent situation** — strong: paper/spreadsheets/"we do it by phone", or a named legacy system they say they are leaving · acceptable: a named modern competitor · weak: bought something in the last 12 months · UNKNOWN: not stated and not resolvable | 0–10 | **GUESSED**, n=1. Northline ran FieldMaster for 15 years and did not buy. That is one data point and it points the wrong way. | The message first; the company's site/job ads second | If long-tenure incumbents are actually the hardest to move, this criterion is inverted and every HOT lead is a 15-year switch that will not happen. **The single most likely thing in this model to be backwards.** |

---

## Intent (0-40)

Everything here is read out of the message itself.

| Criterion | Weight | Provenance | Observable from | If wrong |
|---|---|---|---|---|
| **I1 · States a problem of its own** — strong: named **and** quantified (hours, jobs lost, callbacks, $) · acceptable: named, not quantified · weak: gestures at "efficiency" · **no: asks only for specification and never says what hurts** → also fires the competitor check | 0–10 | **GUESSED.** | The message | A terse owner who writes four lines scores weak. Long messages will out-score short ones — see the falsification list. |
| **I2 · Names the incumbent and why they are leaving it** — strong: names it and the breaking point · acceptable: names it · weak: "looking at options" · no: silent | 0–10 | **GUESSED.** | The message | Someone can be miserable and unable to name the system. Rare. |
| **I3 · The shape of the ask** — strong: wants it on *their* jobs / their data / their dispatcher in the room · acceptable: a demo · acceptable-minus: pricing · weak: general information | 0–10 | **BORROWED** from generic B2B request-type ranking. ⚠️ ServiceGrid has **never answered an inbound lead**, so there is no local evidence that a demo request beats a pricing request. This is the weakest weight in the model. | The message | If pricing requests actually convert better here, this reverses. Cheap to find out after ~40 leads. |
| **I4 · Evidence of internal motion** — strong: lists requirements, names alternatives being evaluated, says the team/board has agreed to change · acceptable: has tried something and says what failed · weak: personal curiosity · no: none | 0–10 | **GUESSED.** | The message | Confuses a thorough writer with an organisation that has decided. |

🔴 **A stated pass/fail condition is not an Intent signal.** "Does it talk to QuickBooks Enterprise?"
is a dealbreaker wearing a question's clothes. It goes in the `condition` column, never into I1–I4.
Enthusiasm about a product that cannot meet the condition is not intent — it is a loss you have not
had yet. **This is the rule Northline was lost to,** on call 1, in March.

---

## Timing (0-20)

| Criterion | Weight | Provenance | Observable from | If wrong |
|---|---|---|---|---|
| **T1 · A dated event in the message** — strong: an actual date, renewal month, season start, audit, contract end, lease · acceptable: a named quarter · **weak: urgency adjectives with no date ("ASAP", "urgently", "excited to move fast")** · no: nothing | 0–10 | **GUESSED**, but the ranking is deliberate: *the trigger is in the nouns and the dates, never in the adjectives.* Adjectives are free and score 3. | The message. **Never UNKNOWN** — you have read the whole message, so absence is an observation. | Some people are genuinely urgent and bad at saying so. |
| **T2 · A trigger in the company record** — strong: new owner/CFO, an acquisition, a new branch, a visible hiring surge for field techs · acceptable: a product/service-line launch · no: looked and found nothing · **UNKNOWN: did not look (research budget spent)** | 0–10 | **GUESSED.** | Company news, the leadership page, **job postings** | Hiring surges are seasonal in this trade; a spring hiring push may be weather, not growth. |

**Budget, deliberately absent from Timing.** Whether money exists this quarter is the thing every
scoring model wants and none can see. A renewal date says money exists *on a schedule*; it does not
say it is available to us. The proxy and the answer are different objects. Budget carries zero
points and lives in the first-call questions.

---

## Tiers

Thresholds are read from **`score_100`**, never from raw points. Owners: ServiceGrid has 3 AEs and
no SDR, so inbound goes to the AE on the weekly rota.

| Tier | score_100 | Next action | Owner | Within |
|---|---|---|---|---|
| **HOT** | ≥ 70 | AE phones them, and opens by answering the condition they stated. If we do not know the answer, say so and give a date. Book the call in the same reply. | AE on rota | **4 business hours** |
| **WARM** | 45–69 | AE replies by email answering their stated problem in their words, and asks **the one** question from `would_change_tier`. No call booked yet. | AE on rota | **1 business day** |
| **COLD** | < 45 | Batched. One reply, useful, no meeting ask: the two most relevant resources and an open door. | AE on rota, **Friday batch** | **weekly** |
| **DQ** | — (rule-fired, never reachable by points) | Send the decline written in the disqualifier table. Name the reason in one line and point somewhere useful. Log the rule that fired. | AE on rota | **2 business days** |

⚠️ **Response times are promises, not aspirations.** ServiceGrid has never answered an inbound lead
— `inbound_volume` is 0. A one-hour SLA would be fiction on day one. Four business hours is what
three AEs with a full outbound quota can actually hold; raise it when the team has held it twice.

🔴 **The floor rule — the hole that "UNKNOWN is not zero" opens.** Dropping unseen criteria out of
the denominator can hand a lead 100/100 on the strength of two visible criteria. So: **if
`score_out_of` < 40, the tier is capped at WARM and confidence is `low`,** whatever the percentage
says. A lead we can barely see is not a hot lead; it is a lead we need to look at.

### Pending tier — HOLD (not yet earned)

`qualify-lead` says an agency or consultant asking for an unnamed client is "a refusal to *score*,
not a refusal to engage". None of the four tiers can express that: DQ declines them, and every other
tier scores them. So HOLD is written here with its action ready —

> **HOLD** · no score · *Reply asking who the client is and what they run today. Do not score, do
> not decline, do not book. Re-enter scoring when the company is named.* · AE on rota · 1 business day

— and it is **switched on the first time a real lead needs it**, not before.

⚠️ **A tension in the method, stated plainly:** Step 0 forbids reading the leads, and the tier rule
says to add a fifth tier only when a real lead in front of you cannot be served by the four. A first
run can never satisfy both at once. A pending tier with a written trigger is the compromise. The
`qualify-lead` run decides.

---

## What this model cannot see

| Unknown | Proxy, if any | The question | Where it is asked |
|---|---|---|---|
| Budget / ~$22k ACV fit | none found in a full sourcing run | "What do you pay for the system you're replacing?" | First call, Q1 |
| Technician count | published employee count · branch count · **job postings (untried)** | "How many people carry a phone into a customer's building on a Tuesday?" | First call, Q2 |
| Actually independent | PE roll-up tracker negative screen — high precision, low recall | "Who owns the company today?" | First call, Q3 |
| Accounting integration | none — **and we do not know our own answer** | "What do you run your books in, and what has to flow into it?" | First call, Q4 |
| Commercial or residential/light-commercial | the language on their own site | "Walk me through your last job." | First call, Q5 |
| Who else says yes | none | "How did you choose the system you're on now?" | First call, Q6 |

Full text with the wording that gets a useful answer: **`signals/qualification/first-call-questions.md`**.

---

## What would prove this model wrong

```
PREDICTED EVENT   a first meeting actually HELD — not booked, not scheduled. Held.
WINDOW            within 30 days of predicted_on
RECORDED BY       a human, in the `outcome` column of leads/qualified-YYYY-MM-DD.csv
```

Chosen because it is observable in days. "Bought" is the event that matters and it is useless here:
a 45-day cycle means nothing is learned until the quarter is over, so the loop never closes.

```
THIS MODEL PREDICTS
  HOT leads reach a held meeting at a materially higher rate than WARM.
  The disqualifiers exclude nobody who would have bought.

IT IS WRONG IF
  · HOT and WARM reach held meetings at the same rate   -> the weights carry no information
  · a DQ'd lead buys from a competitor                  -> a disqualifier is too wide
  · every lead lands within ~15 points of every other    -> it does not discriminate
  · the highest scores are all the longest messages      -> we scored verbosity, not intent
  · every outbound reply lands COLD and every inbound
    form lands WARM or better                            -> we scored the channel, not the lead
  · a tier is empty across the whole batch               -> that tier is decoration
  · a disqualifier fires on nobody, twice running        -> it is not a rule, it is a worry
  · F4 inverts: the long-tenure-incumbent leads all die  -> the switching hypothesis is backwards
```

**Expected spread, so that clustering is detectable:** across a batch of ~14, at least three of the
four tiers should be occupied and the highest and lowest `score_100` should be at least 30 apart.

**CHECK IT AFTER 40 scored leads, or 2026-11-30 — whichever comes first.**

⚠️ **And the honest version of that number:** 14 leads cannot falsify a weight. At ServiceGrid's own
rates (500 "interested" → 145 opportunities → 16 signed), the difference between a HOT and a WARM
conversion rate is not distinguishable in a batch this size. **The first run tests the machinery —
clustering, empty tiers, dead disqualifiers, misaligned columns — not the weights.** Anyone who
reports "the model works" after 14 leads has measured nothing.

---

## Weights I guessed

**Read this section first. It is the whole model.**

| | |
|---|---|
| **GUESSED** (nobody knows; a starting hypothesis) | F1 technician band · F4 incumbent situation · I1 problem stated · I2 incumbent named · I4 internal motion · T1 dated event · T2 company trigger — **7 of 10** |
| **BORROWED** (a benchmark or a stated belief; source named) | F2 independence (Aug-15 tracker screen) · F3 the writer is the buyer (`CLAUDE.md` §1, unchecked) · I3 shape of the ask (generic B2B ranking, contradicted by having zero inbound history) — **3 of 10** |
| **OBSERVED** (derived from closed outcomes in this repo) | **none. Zero of ten.** |

**What would replace the guesses with observations, and roughly when:**

1. **The 16 won deals need records.** They are the only observed evidence this company owns and they
   are not in the repo. One folder each under `customers/`, with size, trade, incumbent, buyer title
   and the reason they signed, converts F1–F4 from GUESSED to OBSERVED in a single afternoon of
   data entry. **This is the highest-value unblocked task in the whole system** and it needs nobody's
   permission.
2. **The 129 lost opportunities** (145 − 16) need a reason each. Loss reasons are what disqualifiers
   are actually made of, and `CLAUDE.md` §1 currently has two.
3. **A job-postings pass** turns F1 from a proxy into something close to a measurement, and feeds T2.
   Named as the best free source in the Aug-15 run and never run.
4. **Forty scored leads with `outcome` filled in by a human** turns I1–I4 and T1 from GUESSED into
   OBSERVED. At zero inbound volume today, that is an outbound-reply number, not an inbound one.

**Open contract question, unresolved and not scorable until someone answers it:** the Aug-15 run
found that the searchable segment is **commercial mechanical contractors**, while `CLAUDE.md` §1
reads residential/light-commercial. Two different buyers, two different pains. Until ServiceGrid
picks one, this cannot be a criterion, and F1's band means different things for each.
