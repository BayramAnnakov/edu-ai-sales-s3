# ServiceGrid — sample sales system

> **This is a fictional company.** Use it when you have no data of your own, or cannot put your
> real data on a laptop. Every exercise in the course works on it.
> Same structure as `sales-starter/` — the rules in section 4–5 below are identical.

## 1 · Who we sell to

**We sell:** ServiceGrid — B2B SaaS for US field-service contractors (HVAC, plumbing, mechanical).
Dispatch, scheduling, job costing, work orders, inventory, a tech mobile app.

**We sell to:** US field-service contractors, 20–200 technicians, owner-operated.
Buyer is usually the owner, CFO or co-owner. ACV ~$22,000/year.

**We never touch:** national franchises (they buy enterprise), under 10 techs (cannot afford it).

**Our motion:** LinkedIn outbound only. One channel, one segment, three AEs, 12 months.
No inbound motion exists.

**Our benchmarks:** we have not established any. See `signals/unrouted/`.

## 2 · Autonomy baseline

| Arrow | What it is | Level |
|---|---|---|
| targeting → replies | outbound | `L2` — an AI SDR sends the connection requests |
| replies → meetings | qualification | `L1` — a classifier labels replies, AEs take it from there |
| meetings → what we know | call intelligence | `L0` — 155 calls logged, none read |
| what we know → targeting | analytics | `L0` — nobody has ever changed targeting because of a call |

## 3 · What is known to be wrong

Nothing has been diagnosed yet. That is the exercise — run `/audit-funnel` on
`leads/funnel-12mo.csv` and see what it says.

⚠️ One thing is worth knowing before you start: **"500 interested" is a classifier output, not a
human observation.** Nobody has checked it.

## 4 · Filing rules

Anything touching one account → `customers/<company>/`. Bulk lists → `leads/` as CSVs.
A lead becomes a customer folder on first real interaction. Every account file carries
`Last updated`. Append, never overwrite. Say what you do not know.

## 5 · signals/ is not a diary

Filed by **what it changes**: `targeting/` (who we go after) · `qualification/` (who we let
through) · `unrouted/` (learned it, don't know what it changes yet).
Anything about one company lives in that company's folder instead.
