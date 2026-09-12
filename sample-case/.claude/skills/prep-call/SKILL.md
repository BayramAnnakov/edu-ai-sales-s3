---
name: prep-call
description: Build a one-page brief for an upcoming sales call — who they are from public sources, what they already told you, what killed the last deal that looked like this, and the questions only this call can answer. Writes customers/<company>/brief-YYYY-MM-DD.md. Use when the user says /prep-call, names a company or person they are about to talk to, or asks what to ask on a call.
---

# Prep Call

One page, read in the two minutes before the call. Not a research report.

> Companion to `debrief-calls`, which reads the conversations after. This skill is where the last
> deal's lesson gets spent — if it does not change what you ask on the next call, nothing was
> learned.

**The rule this skill exists to enforce:** *the most valuable line in a brief is a question you
already know you cannot answer.* Public data tells you who they are. Your own `customers/` folder
tells you what you got wrong last time. The brief exists to stop you repeating it.

---

## Step 1 · Read your own files first — before any web search

Web research is the part everyone does. It is not the part that wins the call.

1. **`customers/<company>/`** — do you already know these people? If there is a folder, read
   `CLAUDE.md`, `TIMELINE.md` and every debrief. This person may be a returning loss.
2. 🔴 **`customers/` — every other folder.** Look for a deal that failed on the same class of
   requirement. If you find one, the question that killed it goes at the top of this brief and
   you carry the answer into the call.
3. `signals/qualification/` — the first-call questions, and anything a debrief routed here.
4. `signals/qualification/scoring-model.md` — what tier the model gave them and why. You are
   about to find out whether it was right.
5. The original inbound message or reply, verbatim. **Read what they actually wrote.** Most of the
   brief is in it and people skip straight to LinkedIn.

---

## Step 2 · Research the outside — capped

🔴 **At most 5 lookups. Stop at 5 even if you have found nothing.** A brief that arrives after the
call is worth zero, and an uncapped research loop is how this skill hangs.

Spend them on things that **change what you ask**, in this order:

1. What they run today — the incumbent system, the accounting package, anything in a job ad.
2. Size and structure — headcount, locations, who owns it.
3. A dated event — funding, acquisition, a new hire in the buying seat, a contract renewal, a
   compliance deadline, a season.
4. The person — role, tenure, whether they are the buyer or the researcher.

For every fact: **the source URL and the date you read it.** No source, no line in the brief.

⚠️ **Say what you could not find.** "Technician count: not published" is a real finding — it goes
in the questions list. An unsourced guess dressed as a fact is worse than a blank.

---

## Step 3 · Write the brief → `customers/<company>/brief-YYYY-MM-DD.md`

🔴 **One page. If it does not fit on a screen, cut the research, not the questions.**

```markdown
# <Company> · <date of call> · <who is on it>

**They wrote:** "<the single most load-bearing sentence from their own message>"

## What we know          (each line: fact — source — date read)
## What we do NOT know   (and which of these only this call can answer)

## 🔴 The question that killed the last deal like this
<the requirement, the deal it killed, the date> — **and our answer today is: <answer, or "we do
not know, and that is what has to change before this call">**

## Ask these
1. …  (each one: the question, and what a good / bad answer means)

## Do not
<what went wrong last time, in one line — e.g. a topic that was pitched over, a number that was
offered instead of an answer>

## If they ask X, we say
<the two or three things they are most likely to ask that we have historically fumbled>
```

**The questions come from three places, and say which:** the first-call questions your scoring
model could not answer from outside · what a past debrief showed you failed to ask · what your
capped research could not find.

---

## Step 4 · Report

Six lines, no more:

1. Where the brief was written.
2. Prior history with this account — **yes/no, and if yes, the outcome and the date**.
3. The one question at the top, and whether your side can answer it today.
4. Lookups used, out of 5.
5. What you could not find.
6. ⚠️ Anything in the brief that came from the scoring model rather than from evidence — that is a
   prediction, and this call is the test of it.

---

## What you are forbidden to write

- ⛔ **A fact without a source and a date.**
- ⛔ **A summary of their website.** Nobody has ever won a call by reciting the About page.
- ⛔ **A "personalisation hook"** — a weather-and-hometown line dressed as research. If it does not
  change a question you will ask, cut it.
- ⛔ **An answer your company has not actually confirmed.** If the answer to the top question is
  "we do not know", the brief says *we do not know*, in those words, so somebody goes and finds out
  before the call instead of improvising on it.
