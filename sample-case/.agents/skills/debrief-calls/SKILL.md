---
name: debrief-calls
description: Read every recorded call on one deal, reconstruct what actually happened, and separate it from what the CRM says happened. Writes customers/<company>/TIMELINE.md, a debrief per call, and routes what it learned into signals/. Use when the user says /debrief-calls, hands over a call transcript or a folder of them, asks why a deal was lost or stalled, or asks what a call should have changed upstream.
---

# Debrief Calls

A CRM knows the **outcome**. It does not know the **reason** — the reason field is a sentence a
human typed afterwards, from memory, about a deal they lost. The recording is the only unedited
record of what was said.

This skill reads the recordings and writes down what the record does not contain.

> Companion to `prep-call`, which carries the result *into* the next conversation.
> `qualify-lead` scores a stranger from outside. This skill scores your own side from inside.

**The rule this skill exists to enforce:** *the outcome is real, the reason is fiction.* Report
what was said, quote it, date it — and mark clearly where you are reading between the lines.

---

## Step 0 · Read the deal before you read the calls

In this order, and say what you found:

1. `customers/<company>/CLAUDE.md` — status, tier at entry, contacts, the stated loss reason.
2. `CLAUDE.md` §1 — the ICP, the price floor, the "We never touch" list.
3. `signals/qualification/scoring-model.md` — the model that scored this deal, if one exists.
   You will need it in Step 5. If it is absent, say so and carry on; Step 5 degrades to a list.
4. The calls themselves — **all of them, oldest first.**

⚠️ **If some calls have no transcript, say how many and move on.** A partial archive is the normal
case. Never invent a call that left no record, and never describe what "probably" happened on it.

⚠️ **Audio, not text?** Transcribe it first, then work from the transcript. Say which tool you used
and that the transcript is machine-made. A name that is spelled two ways is an ASR artefact, not
two people — but say that you made that call.

---

## Step 1 · The unanswered-question ledger — do this FIRST

🔴 **This step exists because it finds an absence, and absences are the thing you will otherwise
miss.** A question that stops being asked leaves no trace anywhere. Do it before you form a view
of the deal, or you will read the transcripts looking for evidence of the view.

Build one table, across all calls:

| # | who asked | what they asked | call(s) asked on | answered? | last time it was asked |
|---|---|---|---|---|---|

Rules:

- **Every question the buyer's side asked**, not just the ones that sound important.
- "Answered" means *a specific answer was given on a call or named as delivered on a later call*.
  "I'll confirm", "I'll send the doc", "our integration layer is robust", "we have an API" are
  **not answers**. Score them `deflected`.
- 🔴 **Then check what happens after the last time each one was asked.** A buyer who asks three
  times and then stops has not been satisfied. They have stopped expecting an answer.
  **Say the call number where each question goes silent, and how many calls came after it.**

---

## Step 2 · What they said they needed, versus what they asked for

These are different things and the difference is most of the value in the file.

- **Requirement** — what they asked for. Usually a product fact. Easy to hear.
- **Need** — the outcome they are trying to produce, and the person they are producing it for.
  Usually said once, early, in a sentence that got interrupted.

For each: quote it verbatim with the call number, and say **whether anyone on your side ever
responded to it.** Count the interruptions if there are any — say the number.

---

## Step 3 · Everyone who appeared, and every date anyone said out loud

Two lists. Both are cheap to build and both are usually missing from the CRM.

**People.** Everyone who spoke or was named as a decision-maker, with the call they first appear
on. Then diff against the contacts table in `customers/<company>/CLAUDE.md` and **name who is
missing**.

**Dates.** Any board meeting, renewal, contract end, audit, season, budget cycle or deadline that
anyone said. Quote it and give the call number. Then say whether it appears anywhere in the record.

⚠️ A date spoken on a call is the same evidence as a date written on a form. Your scoring model
almost certainly reads dates off written messages only.

---

## Step 4 · Score the calls, and say what the score does not measure

Use the rubric in `signals/qualification/call-rubric.md` if one exists. If not, use **SPIN** and
say that you chose it: Situation / Problem / Implication / Need-payoff, plus talk ratio.

Per call: a 1–5 discovery score, the talk ratio if you can estimate it, and **one quoted line** as
the evidence for the score. One line, not a paragraph.

🔴 **Then write this sentence in your output, in your own words:** the rubric measures whether
your side followed a playbook. It does not measure whether the deal was winnable. A rep can score
better on every call and lose the same deal, if what was missing was a fact nobody went and got.

⛔ **Never name a real salesperson as a poor performer in a file that gets shared.** Describe the
behaviour, not the person. If the transcript names them, use the role.

---

## Step 5 · What this should change upstream — the only step that pays

Everything above is description. This is the part that changes something.

For each finding, name **the exact edit** and **where it goes**:

| what the calls showed | what changes | which file |
|---|---|---|
| a question asked N times, never answered | a field on the intake form **and** an answer your company does not yet have | `signals/qualification/` |
| a stakeholder who appeared mid-deal | a contact row, and a question on the first call about who else decides | `customers/<company>/CLAUDE.md` |
| a date said out loud | a timing criterion that can read speech, not just forms | `signals/qualification/` |
| a competitor named | who you actually lose to, and why | `signals/targeting/` |
| a product gap | the product team, and a disqualifier if you cannot close it | `signals/unrouted/` |

**Write the signal file.** One file, `signals/<bucket>/<slug>-YYYY-MM-DD.md`, with: what was heard,
the quote, the call, the edit proposed, and who has to decide.

🔴 **Before you write it, go looking for who is asking the same thing TODAY. Search two places
and say what you found in each:**

1. **`customers/`** — every folder. Another account that hit the same wall.
2. **`leads/`** — every inbound and reply file, including the most recent one. **This is where a
   live deal actually lives before anyone opens a folder for it**, so it is the one that matters
   and the one that gets skipped.

Grep for the *thing*, not the company — the product name, the system, the condition. Then name the
person, the file and the date, and put them at the top of the signal.

If both come back empty, **say "nobody is asking this today"** rather than staying silent. A
negative result you looked for is evidence; one you never checked is not.

⚠️ This is not a nice-to-have. A debrief on a dead deal that does not surface a live one is
history. The point of reading old calls is the deal you can still save.

---

## Step 6 · Write the files

**`customers/<company>/TIMELINE.md`** — append, never overwrite. Oldest first:

```
## YYYY-MM-DD · call N of M · <duration> · <who was on it>
**What they asked for:** …
**What was promised:** …
**What was actually delivered by the next call:** …   ← usually the interesting row
**Open after this call:** …
```

**`customers/<company>/calls/<date>-debrief.md`** — one per call, short. Score, the one quoted
line, what opened, what closed.

**Update `customers/<company>/CLAUDE.md`** — contacts you found, and, if the stated loss reason is
contradicted by the calls, add a line saying **both**: what the field says, and what the calls say.
⛔ **Do not delete the original reason.** The disagreement is the finding.

---

## Step 7 · Report

Print, in this order, and keep it short enough to read on a screen:

1. **Calls read / calls that exist** — e.g. "7 of 10; three have no transcript".
2. **The unanswered-question ledger** — the table.
3. **The call where the deal actually ended**, and how many calls came after it. Say plainly that
   this is your reading, and give the evidence.
4. **What the CRM says versus what the calls say** — two lines, side by side.
5. **The signal you routed**, and the open deal it applies to today, if any.
6. **What you could not tell** from the transcripts.

---

## Cost control — read this before you start

- **Do not research the company on the web.** Everything you need is in the transcripts and the
  repo. If you think you need a lookup, you are answering a different question.
- **A deal is at most ~20 calls.** If there are more, do the ten most recent and say so.
- **One pass per call.** Do not re-read a transcript to polish a sentence.

## What you are forbidden to write

- ⛔ **What was said on a call that has no transcript.**
- ⛔ **That the deal would have been won.** You do not know that. The honest claim is narrower and
  stronger: the blocking fact was available on call 1 and the calls that followed did not go and
  get it. A fast clean loss can be the correct outcome.
- ⛔ **A `reason lost` of your own** in place of the recorded one. Add; never replace.
- ⛔ **A number you did not count.** If you estimate a talk ratio, say it is an estimate.
