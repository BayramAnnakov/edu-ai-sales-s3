# AI Sales Autonomy — Cohort 3

**1 August → 10 October 2026 · 6 × 2h biweekly · 16:00–18:00 MSK · live + recorded**

## The anchor

**Build the place your sales system writes to — then fill it with agents.**

Every session adds one agent to the *same* folder. By 10 October each of you demos five minutes of
something running on your own screen. Not slides.

The reason the course is shaped this way: cohort 2 built a different thing, in a different tool,
every session — and shipped nothing anyone could show. Nothing accumulated because nothing was
the same artifact twice. This cohort has one artifact, and it grows.

## The six sessions

| # | Date | Arrow | What you add |
|---|------|-------|--------------|
| 1 | 1 Aug | the container | `~/sales/` + a funnel auditor that answers a lead |
| 2 | 15 Aug | targeting → replies | ICP + lead search |
| 3 | 29 Aug | replies → meetings | inbound qualification, Fit + Intent + Timing |
| 4 | 12 Sep | meetings → what we know | meeting prep + call analysis |
| 5 | 26 Sep | what we know → targeting | analytics, pipeline, sleeping deals |
| 6 | 10 Oct | the whole system | playbook + **everybody demos** |

## What's in this repo

| | |
|---|---|
| `sales-starter/` | **Copy this to `~/sales/`.** The scaffold every session fills. Its `CLAUDE.md` is the contract your agents follow — read it first; it is the actual product here. |
| `sample-case/` | **ServiceGrid** — a complete fictional B2B SaaS company. Use it if you have no data of your own, or cannot share the data you have. Every exercise works on it. |
| `session1/` … | Slides, notes and homework for each session — **published after that session is delivered**, not before. |

## Start here

```bash
git clone https://github.com/BayramAnnakov/edu-ai-sales-s3.git
cp -r edu-ai-sales-s3/sales-starter ~/sales
cd ~/sales && claude        # or: codex
```

Then read `~/sales/CLAUDE.md` and fill in section 1.

**You need one of Claude Code or Codex — either is enough.** Two different vendors, two different
payment rails; if one will not take your card, try the other.

```bash
npm install -g @anthropic-ai/claude-code     # claude --version
npm install -g @openai/codex                 # codex --version
```

## Using the sample case

No data of your own, or data you cannot put on a laptop? Everything works on ServiceGrid:

```bash
cp -r sample-case ~/sales-demo
cd ~/sales-demo && claude
```

Demoing on the sample case in session 6 is fine. Just say which you used. The rule is that the
agent has been **run**, not that the data is proprietary.

## Definition of done — 10 October

1. It lives in `~/sales/skills/`
2. It runs with one command
3. It has processed **10 objects** — yours or the sample case
4. `signals/` contains lines **it wrote itself**

---

Questions → the Telegram group. Recordings, slides and notes land in each session folder
after that session runs.
