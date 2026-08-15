# A finished run — to read, not to start from

This is what `find-leads` produced on ServiceGrid's own ICP, on 15 August 2026, using nothing but
web search. It is here for two reasons:

1. **If your own run fails, is slow, or you never got the skill installed** — audit these rows
   instead. The exercise is identical and nobody sits it out.
2. **If your run worked** — compare. Different runs find different companies and disagree about
   what could be sourced. That disagreement is itself the lesson.

⚠️ **Do not copy these files into `leads/` or `signals/`.** The skill appends to existing files,
and mixing this run's rows with yours produces a list where you cannot tell which row came from
where. That is precisely the failure this session is about.

## What is here

| file | what it is |
|---|---|
| `leads-2026-08-15.csv` | 5 scored rows. `evidence_checked` is `no` on every one — the agent is forbidden from setting it. |
| `drafts-2026-08-15.md` | three outreach drafts. Each opener traces to that row's verified evidence. |
| `icp-gaps-2026-08-15.md` | what could not be sourced, and the verdict for each: proxy, call, or delete. |

## The finding worth reading first

Open `icp-gaps-2026-08-15.md` and look at the ownership row. Several companies describe themselves
as family-owned on their own website while being owned by someone else. **A company's About page
is not an ownership record** — and "owner-operated" is a criterion almost every ICP in this
industry contains.
