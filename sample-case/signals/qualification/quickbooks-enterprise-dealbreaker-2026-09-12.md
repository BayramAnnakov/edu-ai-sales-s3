# The accounting-posting condition — asked 9 times on a dead deal, and by 3 live ones today

**Filed:** 2026-09-12 · **Bucket:** `qualification` — it changes who we let through, and what we
must answer before we let them.
**Source:** all ten call transcripts, `customers/northline-mechanical/calls/`, deal #2291,
12–26 March 2026. See `customers/northline-mechanical/TIMELINE.md`.

---

## 🔴 Who is asking this TODAY

I searched two places. Both came back with live names.

### `leads/` — three open leads, none answered, one past its own deadline

| Who | Where | Said | The condition |
|---|---|---|---|
| **Denise Okonkwo**, co-owner, Vantage Mechanical (120 techs, commercial HVAC + controls, Denver / Colorado Springs) | `leads/inbound-2026-08-28.md` §8 · **2026-08-28** | *"We run QuickBooks Enterprise on the accounting side and that isn't moving… is that a real integration or a CSV export? **That question has ended two vendor conversations for us already.**"* | QBE, real posting vs CSV |
| **Lisa Grant**, CFO & co-owner, Northline Mechanical — **the same buyer, back** | `leads/inbound-2026-08-28.md` §1 · **2026-08-24** | *"we spoke with one of your people back in the winter… FieldMaster just told us they're ending support for the on-prem version in 2027 and we'd have to move… **Same question I had last time.**"* | QBE Desktop, hosted — posts or exports |
| **Dana Whitfield**, Harbor Plumbing (55 techs, 3 branches, Pacific NW) | `leads/inbound-2026-08-01.md` · **2026-08-01** | *"does it talk to Sage? That's what accounting runs on and **they won't move off it**."* | Same shape, different system |

Adjacent, same pain without naming a system — **Angela Boysen**, Keystone Mechanical (60 techs,
commercial, two states), `leads/inbound-2026-08-28.md` §6, 2026-08-27: *"we can't close out a job
and know what it actually cost us until the accountant gets to it weeks later."*

🔴 **Denise Okonkwo is the most qualified lead in the file and her clock has run out.** She wrote on
28 August asking to get her controller on a call *"in the next two weeks"* — **that window closed
on 11 September, yesterday.** Her contract renews **12 November** and she has already decided not to
renew, so she is buying something. There is no record in this repo of a reply to her, to Lisa, or to
Dana.

### `customers/` — one folder exists, and it is Northline's

No other account has hit this wall, because no other account exists. 16 companies signed in the last
twelve months and **not one has a folder** (`signals/qualification/scoring-model.md`). So the
`customers/` search is not a negative result — it is a search across a directory with one row in it.
**Whether our 16 won customers run QuickBooks Enterprise is unknown and unasked**, and it is the
cheapest way to settle this question that exists: ask the 16 people who already bought.

---

## What was heard

**The question, asked nine times across six consecutive calls, answered zero times.**

| # | Call | Date | Verbatim | Response |
|---|---|---|---|---|
| 1 | 1 | 03-12 | *"Do your reports integrate with QuickBooks Enterprise? That's what accounting runs on."* | *"I believe we support it, but let me confirm… our integration layer is very robust."* |
| — | 1 | 03-12 | *"that's really the dealbreaker for us. **If it doesn't talk to QuickBooks Enterprise, it's a non-starter.**"* | *"Absolutely, I'll get you that answer. But let me also mention pricing—"* |
| 2 | 2 | 03-13 | *"Did you get an answer on QuickBooks Enterprise?"* | *"I've got a request in with our solutions team."* |
| 3 | 3 | 03-16 | *"does this post to QuickBooks Enterprise, or does it export a file that somebody re-keys?"* | *"our integration layer is really robust. We've got a full REST API, webhooks—"* |
| 4 | 3 | 03-16 | *"Mike. Posts, or exports?"* | *"Let me get you the integration doc."* — **never sent, never mentioned again** |
| 5 | 4 | 03-17 | *"Can he answer the QuickBooks question?"* | *"That's exactly the kind of thing he's for."* |
| 6 | 5 | 03-18 | *"When a job closes in your system, does the cost and revenue post into QuickBooks, or does somebody export a file and import it?"* | *"we have a fully open REST API."* |
| 7 | 5 | 03-18 | *"Do you have a QuickBooks Enterprise connector, today, that your customers use?"* | *"We have customers integrated with QuickBooks, yes."* |
| 8 | 5 | 03-18 | *"Enterprise, or Online?"* | *"I'd have to check which edition."* |
| 9 | 6 | 03-19 | Controller: *"I've got a reconciliation problem every month unless it posts."* + CFO: *"That's the same question I've been asking, Mike."* | *"Ravi's confirming."* |

**Then it stops.** Call 6, 19 March, is the last call on which anyone at Northline asked anything.
Four calls followed. Their word count per call: **148 → 23 → 26 → 30 → 26.**

A buyer who asks nine times and then stops has not been satisfied. She has stopped expecting an
answer.

---

## The part that is about us, not about them

**We still do not know our own answer.** Not "we said no" — *we never found out.* Two people on our
side committed to confirming it (call 5, call 6) and neither confirmation appears anywhere in the
archive or the repo. The `scoring-model.md` already records this under what the model cannot see:
*"Accounting integration — none — **and we do not know our own answer**."*

⚠️ **And an artefact nobody in this repo has read.** Denise Okonkwo writes: *"**I saw on your site
that you integrate with it**."* That is her report, not a verified fact — but if ServiceGrid's
marketing site claims a QuickBooks Enterprise integration that five calls could not substantiate,
the claim is generating exactly the leads we cannot serve, and the fix is not in any codebase.
**Somebody has to open the website and read it.**

---

## The edits this proposes

| What the calls showed | What changes | Which file | Who decides |
|---|---|---|---|
| A pass/fail condition asked 9× and never answered | **Answer it.** One question to engineering: *does a QBE Desktop (hosted) connector exist today that posts job cost and revenue, and do any customers run it?* Then write the answer down where a rep can reach it in 30 seconds. | product / engineering — **not a sales artefact** | Head of product |
| The same condition arrives in writing, unprompted, from 3 of 9 live leads | A **`condition` field on the intake form and in the qualified CSV** — verbatim, pass/fail, scored zero. `scoring-model.md` already says a stated condition *"goes in the `condition` column, never into I1–I4"*. **The column does not exist yet.** | `signals/qualification/scoring-model.md` + `leads/qualified-*.csv` | Sales lead |
| Enthusiasm on calls 3–4 while the condition sat open | **A HOT tier cannot be reached while a condition is unanswered.** Today's HOT rule says the AE opens by answering the condition; it does not say what to do when we *cannot*. Proposed: unanswered condition ⇒ capped at WARM, same as the `score_out_of < 40` floor rule. | `signals/qualification/scoring-model.md`, Tiers | Sales lead |
| If the answer is "no, and not soon" | Then *"runs QuickBooks Enterprise Desktop"* is a **disqualifier**, and it belongs in `CLAUDE.md` §1 under "We never touch" with the other two. **Do not add it until engineering answers** — a disqualifier written from a guess costs more than a lost call. | `CLAUDE.md` §1 | ServiceGrid owner |
| A dated event said out loud — *"the board meets on the ninth of April"* (call 4) — and never recorded | **T1 must be able to read speech, not only written messages.** The deal was booked lost on 28 March, 12 days before the only dated event in it. | `signals/qualification/scoring-model.md`, T1 | Sales lead |
| A second condition from the controller — burden per crew vs company-wide, 38%, install ≠ service (call 6) | First-call question 4 asks *"what do you run your books in"*. It does not ask **what has to be inside the margin number**. Add it. | `signals/qualification/first-call-questions.md` | Sales lead |
| A competitor volunteered and never pursued — *"someone the industry association recommended"* (call 6) | We do not know who we lose to. **Propose `signals/targeting/who-the-associations-recommend.md`** — not written here, because one unnamed clause is not yet a finding. Ask Lisa now; she is back and she will tell you. | `signals/targeting/` (proposed) | Sales lead |
| Two named deciders — *"My co-owner. And operations."* (call 8) — never named or met | Contact rows, and a first-call question about the last decision. Done today for Northline. | `customers/northline-mechanical/CLAUDE.md` | AE on rota |

---

## What to do this week, in order

1. **Get the engineering answer.** Everything else is blocked on one question that has been open
   since 12 March.
2. **Reply to Denise Okonkwo.** Her stated window expired yesterday and her renewal is 12 November.
   If the answer is no, tell her it is no — she has already ended two vendor conversations on this
   and will respect the fourth minute more than the fourth call.
3. **Reply to Lisa Grant.** She came back on her own, with a forced migration behind her. The
   scoring model's own disqualifier table already names her: *"Already in `customers/` → route it to
   the AE who owns the folder, and **answer the question that killed it last time before anything
   else.** Today that is exactly one company: Northline Mechanical."*
4. **Ask the 16 won customers what they run their books in.** It converts this whole file from one
   lost deal's anecdote into a number.

---

## What this file does not claim

⛔ **Not that Northline would have been won.** FieldMaster had 15 years of tenure and the answer may
well have been "no, we do not post to QuickBooks Enterprise" — in which case the correct outcome was
a fast, clean loss on **call 1**, not a slow one on call 10 after a 12% discount against an obstacle
the buyer twice said did not exist.

The narrow claim, which is enough: **the blocking fact was available on call 1, it was asked for
nine times, nobody went and got it, and it is still not known today while three live leads wait on
the same answer.**
