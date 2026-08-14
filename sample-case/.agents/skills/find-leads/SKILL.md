---
name: find-leads
description: Find and score outbound leads against the ICP in this repo's CLAUDE.md. Writes a scored CSV to leads/ and files what it could not source to signals/targeting/. Every score carries the one source you would open to check it.
---

# Find Leads

Turn the ICP in `CLAUDE.md` into a searched, scored, **auditable** lead list.

> Adapted for this course from the `lead-search` plugin
> (github.com/BayramAnnakov/lead-search-plugin). This version reads *your* contract, files into
> *your* folders, and refuses to mark its own work as verified.

**The rule this skill exists to enforce:** a score you generated is not evidence that the score is
right. Two production lead-gen engines have each shipped records that disagreed with reality. So
every row carries the one link a human would open — and only a human may mark it checked.

---

## Step 0 · Load the ICP from the contract

Read `CLAUDE.md` section 1 — **"Who we sell to"** and **"We never touch"**. That is the ICP.
Also read anything in `signals/targeting/` — that is what previous runs and audits already learned.

If section 1 is still a `<!-- fill in -->` placeholder, **stop and say so.** Do not invent an ICP
and do not proceed from the prompt alone. Ask for two lines: who you sell to, who you decline.

If the user named an account list to enrich instead of a search to run, skip Step 2 and score
that list — Steps 3-6 are unchanged.

---

## Step 1 · Restate the ICP as a query, and get it approved

Print, before searching anything:

```
SEARCHING FOR
  company:  [industry] · [size band] · [geography] · [other observable attributes]
  person:   [titles, including the variants you will actually search]
  bonus:    [signals that raise the score but are not required]
EXCLUDING
  [from "We never touch"]

CANNOT BE SEARCHED — flagging now, not after
  [criteria with no observable source, e.g. "uses Sage internally", "culture fit",
   "growing fast" — say WHY each one cannot be sourced]
```

**Wait for approval.** An ICP criterion that cannot be sourced is not a criterion, it is a wish —
and the user needs to know which of theirs are wishes *before* the search, not after.

Expand title strings yourself. Literal "VP Sales" under-recalls badly; the real titles include
`VP, Revenue` · `SVP Sales` · `CRO` · `Head of Revenue` · `Commercial Director`. Say which
variants you searched.

---

## Step 2 · Search

Use whatever is available, in this order, and **name which one you used**:

1. a LinkedIn/Crunchbase/company-data MCP, if one is connected
2. web search
3. if neither: stop and say so — do not fabricate companies

Search **company-first, then people**, whenever the segment is niche. Vertical keywords rarely
appear in a person's title.

Cast wider than the target count — you will lose rows in Step 3.

---

## Step 3 · Verify before scoring

For each candidate, resolve **the one URL a human would open to check this row**. Prefer a source
the user can actually reach: a company page, a press release, a filing. Do not make a LinkedIn URL
the only evidence — some of this course's participants cannot open LinkedIn at all.

Drop candidates you cannot evidence. A row you could not verify is not a lead, it is a guess.

---

## Step 4 · Score 1-10, and show the arithmetic

State the criteria and what each is worth **before** the table. For every lead, say which criteria
were **verified**, which were **assumed**, and which **could not be sourced**.

A lead that meets every criterion but whose evidence you could not resolve does not score 9.

---

## Step 5 · Write the list → `leads/leads-YYYY-MM-DD.csv`

Bulk lists go to `leads/` as CSV. **Never one folder per lead** — a lead earns a folder in
`customers/` only on first real interaction.

```
name,title,company,headcount,location,score,evidence_url,evidence_checked,unsourced,trigger,outreach_angle,source_tool,pulled_on
```

- **`evidence_url`** — the ONE link to check this row. Never a search-results page.
- **`evidence_checked`** — **always write `no`.** Only a human who has opened the URL may change
  it to `yes`. **You may never set this yourself**, in this run or any later one.
- **`unsourced`** — semicolon-separated ICP criteria you could not verify *for this lead*.
  Leaving it empty asserts you checked everything; make sure that is true.
- **`source_tool`** — what you actually searched with. If you used a fallback, say so here.

Append if the file exists. Never overwrite.

---

## Step 6 · File what you could not source → `signals/targeting/`

A criterion you could not source for most leads is a finding about **the ICP**, not about those
leads. It changes who you go after, so it belongs in `signals/targeting/`.

Write `signals/targeting/icp-gaps-YYYY-MM-DD.md`:

```markdown
# ICP gaps — YYYY-MM-DD

**Query run:** [the ICP as searched]
**Searched with:** [tool] · **Candidates seen:** [N] · **Survived verification:** [M]

| Criterion | Sourced for | Verdict |
|---|---|---|
| [criterion] | [2 of 12] | [cannot be sourced from public data / needs a proxy / fine] |

**Therefore:** [drop it · replace it with a sourceable proxy · accept it is only confirmable
on a call]

**Also learned:** [anything about the segment itself — thinner than expected, consolidated,
no buyer at this size, titles differ from assumed]
```

Then **tell the user which criterion to delete from `CLAUDE.md` section 1**, and offer to edit it.
A signal that never changes the contract is a diary entry, not a loop.

---

## Step 7 · Report

- the two file paths you wrote, with row counts
- **which criteria could not be sourced** — this is the headline, not the leads
- the single lead to contact first, and why
- anything you flagged as ambiguous and did not resolve

Never claim you verified something you inferred. «Скажи, чего ты не знаешь» applies here more
than anywhere: at outbound volume, a confident wrong row costs a person you cannot approach twice.
