# Sales system — the contract

> Copy this whole folder to `~/sales/`. This file is not a readme. It is the **contract**:
> the rules your AI agents follow when they file things. The folders are only where it lands.
>
> Both Claude Code and Codex read this file automatically when working in or below this folder.
> Keep it short — it is loaded on every turn.

---

## 1 · Who we sell to

**We sell to:** <!-- fill in: industry, size, geography, role you sell to -->

**We never touch:** <!-- fill in: segments, geos, company types you decline. Loss reasons live here too. -->

**Our benchmarks:** <!-- fill in as you learn them. A benchmark with no source is an opinion. -->

---

## 2 · Autonomy baseline — session 1

Score each arrow 0–5. `L0` nobody does it · `L1` AI suggests, you decide · `L2` AI acts, you
approve · `L3` AI runs it, you step in · `L4` autonomous in guardrails · `L5` end to end.

| Arrow | What it is | Level | Dated |
|---|---|---|---|
| targeting → replies | outbound | `L_` | |
| replies → meetings | qualification | `L_` | |
| meetings → what we know | call intelligence | `L_` | |
| what we know → targeting | analytics | `L_` | |

**This is your baseline. You diff it in session 6.** Do not edit the old numbers — add a new dated row.

---

## 3 · The contract — session 6

**I am shipping the arrow:** <!-- name one --> · **by 10 October 2026**

Done means: it lives in `.claude/skills/`, runs with one command, has processed **10 objects**
(yours if you can, the sample case if you cannot — say which), and `signals/` contains lines
**it wrote itself**.

---

## 4 · Filing rules — agents follow these without being asked

**Anything that touches one account** goes in `customers/<company>/`, creating the folder if it
does not exist. Never in `leads/`.

**Bulk lists** — lead-finder output, scraped lists, imports — go in `leads/` as CSVs.
**Never one folder per row.** A lead becomes a customer folder on **first real interaction**
(they replied, you spoke, a meeting happened). Without this rule you get 200 near-empty folders
and the twenty that matter disappear.

**Every account file carries `Last updated: YYYY-MM-DD`.** A current-state file that does not say
when it was written is a liar with a straight face — and the state file, not the history file, is
always the one that starts lying. When you append history, refresh the state in the same motion.

**Never overwrite. Append, and date it.**

**Say what you do not know.** A number with no source is an opinion and must be labelled one.

---

## 5 · signals/ is not a diary

Every signal is an **arrow**: you learned something, and something upstream must change because
of it. So signals are filed by **what they change**, not by what they are about.

| Folder | What goes there |
|---|---|
| `signals/targeting/` | changes **who** we go after — segments, triggers, exclusion rules, ICP |
| `signals/qualification/` | changes **who we let through** — scoring criteria, tiers, disqualifiers |
| `signals/unrouted/` | you learned it and do not yet know what it changes |

Anything specific to one company goes in **that company's folder**, not here. `signals/` is only
for things bigger than any single account.

> **`signals/targeting/` is the slow loop — the one that takes a quarter to close and that nobody
> owns.** It will be empty today. If it is still empty in October, your learning loop never closed.
> That is the single most likely outcome, and it is the reason this course exists.
>
> A growing `unrouted/` is its own diagnosis: you are learning things and not turning them into
> changes.

---

## 6 · Where things go

```
CLAUDE.md              this contract
leads/                 bulk lists, CSVs
customers/<company>/
    CLAUDE.md          state · contacts · next action · last updated
    TIMELINE.md        append-only history        (arrives session 4)
    calls/             raw transcripts, never summaries
signals/
    targeting/         → who we go after
    qualification/     → who we let through
    unrouted/          → not yet routed
.claude/skills/        every agent from every session
.claude/commands/      /audit-funnel and friends
pipeline.md            what is in play, whose next action is overdue
                       DERIVED — never hand-written  (arrives session 5)
```

Coming later, once there is something to enforce: `.claude/hooks/` for linter-style checks that
fail a commit when an account file is missing its mandatory fields, and `.claude/agents/` for
subagent definitions. Not created yet — an empty folder teaches nothing.

---

## 7 · Raw, never summarised

`calls/` holds **raw transcripts**. Every new experiment needs a new rubric, and you cannot get a
rubric out of somebody else's summary. A recorder that will not give you the raw transcript is the
wrong recorder.
