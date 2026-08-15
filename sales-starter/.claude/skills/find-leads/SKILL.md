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

---

## Step 1b · Source the unsourceable — do not stop at "cannot"

**Naming a criterion unsearchable is half the job.** Most of them are still *observable*, just not
in the obvious place — because at some point somebody was required to **register, file, publish,
hire, certify, or get reviewed.**

For every line under CANNOT BE SEARCHED, propose the best available **proxy**, and say what it
costs in fidelity:

```
CRITERION            [the wish]
  DIRECT SOURCE      none
  PROXY              [what you will actually look at]
  LIVES IN           [which kind of source]
  FIDELITY           high / medium / low — and what it gets WRONG
  VERDICT            proxy · call · delete
```

**Where to look, by family.** Run down this list before concluding nothing exists:

| family | what it reveals | examples |
|---|---|---|
| **licensing & regulatory registries** | capability, equipment, **permission to do the unusual thing** | aviation waivers and exemptions · transport/carrier censuses · state contractor & trade licence boards · professional registers |
| **procurement & tender records** | budget, buying behaviour, incumbent vendor | government tenders, public contract awards |
| **job postings** | what hurts **right now**, tools in use, team shape | the company describing its own pain, in its own words |
| **corporate filings & registries** | size, revenue, ownership, legal form | SEC · Companies House · national business registers |
| **certifications & memberships** | standards, industry, trade body | ISO, accreditation bodies, industry associations |
| **technology footprint** | stack, integrations | integration/partner pages, tools named in job ads, public repos |
| **reviews & reputation** | who their customers actually are, volume | employer and product review sites, local listings |
| **physical footprint** | scale, geography, capacity | branch and location pages, fleet size, service-area maps |

**Worked example — the move this step exists to teach.**

ICP: *construction, utilities, mining and engineering firms that operate drones.* There is no
"uses drones" field in any company database. But flying commercially **outside the routine rules** —
beyond visual line of sight, over people, over moving vehicles, at night — requires a **waiver**,
and the regulator **publishes every waiver it grants**. Each certificate reads:

```
CERTIFICATE OF WAIVER AND AUTHORIZATION ISSUED TO
  <the organisation>
  Responsible Person: <a named individual>
  ADDRESS: <where they operate>
LIST OF WAIVED REGULATIONS
  <which rules they were allowed to break>
```

Three things fall out of one free public list:
1. **the company** — the certificate is issued to the organisation, even where the filename is a person;
2. **a named person** who owns the programme — your actual contact;
3. **how serious the programme is** — a beyond-line-of-sight waiver is a different buyer from a
   one-off photography permission. *The waived rule is the intent signal.*

⚠️ **And what it gets wrong, which is part of the answer:** it only lists operators who needed a
waiver. Routine daylight line-of-sight flying requires none — so **absence from the list is not
evidence that a company does not fly drones.** It is a high-precision, low-recall source: everyone
on it qualifies, and plenty who qualify are not on it.

A second one, produced by this skill on the sample case: for *"20-200 technicians"* — a number
nobody publishes — a national carrier census carries power units and driver counts by company.
Same shape, same caveat: registration binds only above a size threshold, so absence is not evidence
of being small.

Ask that question of every dead criterion: **who made them write this down, and can I actually
read it?**

**Search for the source. Do not recall it.** Before proposing any proxy, run an actual search for
it — *"<criterion> registry"*, *"<industry> licence lookup <state>"*, *"who publishes <thing>"*,
*"<segment> association member directory"*. The catalogue above is a prompt for where to look, not
a list of answers. Sources differ by country, industry and year, and the one that fits this ICP is
usually not the famous one.

⚠️ **A named registry is not a sourced criterion.** Before you write PROXY, say which specific
record you would look up, and confirm you can actually reach it. If you cannot, write
**`PROXY UNVERIFIED`** and say why — wrong jurisdiction, paywalled, no coverage for this segment,
or you simply could not open it. **Naming a plausible-sounding database is the exact failure this
step exists to prevent.** Registries have jurisdictions and thresholds: a national register only
covers its own country, and most have a size or activity floor below which nobody appears at all.

**Then give each criterion one of three verdicts — never just "unsourceable":**

1. **PROXY** — a different observable stands in. Say what it gets wrong. *(A fleet count is not a
   technician count. An employee band counts office staff. Say so per lead.)*
2. **CALL** — only a conversation resolves it. It is not a targeting criterion at all; it is a
   **qualification question**. Move it out of the ICP and into the first call.
3. **DELETE** — it does not discriminate between candidates, or it passed everything it touched.
   A filter that excludes nobody is identical to not having the filter.

⚠️ **Verdict 3 is the one people miss.** If a criterion passed every candidate you checked, it did
no work. Say that plainly.

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
name,title,company,headcount,location,score,evidence_url,evidence_claim,evidence_checked,unsourced,trigger,outreach_angle,source_tool,pulled_on
```

- **`evidence_url`** — the ONE link to check this row. Never a search-results page.
- **`evidence_claim`** — in three or four words, **which claim that URL actually supports**
  (`ownership`, `headcount`, `the trigger event`). A row carries several claims; one link almost
  never covers all of them, and naming the claim is what stops one opened link from masquerading
  as a verified row.
- **`evidence_checked`** — **always write `no`.** It means, and only means: *a human opened
  `evidence_url` and confirmed the claim named in `evidence_claim`.* It does **not** mean the score
  is verified. **You may never set this yourself**, in this run or any later one.
- **`unsourced`** — semicolon-separated ICP criteria you could not verify *for this lead*.
  Leaving it empty asserts you checked everything; make sure that is true.
- **`source_tool`** — what you actually searched with. If you used a fallback, say so here.

Append if the file exists. Never overwrite.

---

## Step 5b · Draft the top three — from the evidence, not from a template

Write `leads/drafts-YYYY-MM-DD.md`: three short outreach drafts, for the three highest-scoring
rows **whose evidence you could actually resolve**. Not the top three by score — the top three you
can stand behind.

**Each draft opens with the thing you verified.** That is the whole rule. If `evidence_claim` says
*"80+ patents"*, the opener refers to the patents. If the only verified fact is a funding round,
the opener refers to the round. **A draft whose opener is not traceable to that row's evidence is
a template with a name pasted into it, and it will read as one.**

```markdown
## <name> · <company> · score <n>
**Verified:** <the evidence_claim, and the link>
**Could not verify:** <the unsourced list — do not write around it, write within it>

<3-4 sentences. First sentence names the verified fact. One ask at the end.>
```

Three constraints, each of which exists because its opposite has been measured to fail:

1. **Never open with a compliment about the company.** It is the most common opener and it is
   indistinguishable from every other one they received this week.
2. **Never write "no pitch" or "just sharing, no agenda".** Saying it signals a pitch; a real
   outreach test killed that phrasing three times over.
3. **Do not claim what you could not source.** If you could not verify their size, the message
   must not imply you know it. The `unsourced` column is a list of things you may not assert.

⚠️ **Draft only. Never send, never schedule, never open a mail client.** The human sends, or does
not.

---

## Step 6 · File what you could not source → `signals/targeting/`

A criterion you could not source for most leads is a finding about **the ICP**, not about those
leads. It changes who you go after, so it belongs in `signals/targeting/`.

Write `signals/targeting/icp-gaps-YYYY-MM-DD.md`:

```markdown
# ICP gaps — YYYY-MM-DD

**Query run:** [the ICP as searched]
**Searched with:** [tool] · **Candidates seen:** [N] · **Survived verification:** [M]

| Criterion | Sourced for | Proxy used | Verdict |
|---|---|---|---|
| [criterion] | [2 of 12] | [what stood in, or —] | **proxy · call · delete** |

**Therefore, per criterion:**
- **proxy** → name the proxy, where it lives, and what it gets wrong
- **call** → it is a qualification question, not a targeting criterion. It belongs in the first call.
- **delete** → it did not discriminate. If it passed everything it touched, say so.

**Also learned:** [anything about the segment itself — thinner than expected, consolidated,
no buyer at this size, titles differ from assumed]
```

Then **tell the user which criterion to delete from `CLAUDE.md` section 1**, and offer to edit it.
A signal that never changes the contract is a diary entry, not a loop.

---

## Step 7 · Report

- the three file paths you wrote, with row counts
- **which criteria could not be sourced** — this is the headline, not the leads
- the single lead to contact first, and why
- anything you flagged as ambiguous and did not resolve

Never claim you verified something you inferred. «Скажи, чего ты не знаешь» applies here more
than anywhere: at outbound volume, a confident wrong row costs a person you cannot approach twice.
