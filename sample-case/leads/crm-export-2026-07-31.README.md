# CRM export — 31 July 2026

Every opportunity **created** in the CRM between **1 August 2025 and 31 July 2026** — the same twelve
months as `funnel-12mo.csv` — exported on **31 July 2026**. Nothing after that date is in it; anything
in `leads/` or `customers/` dated after 31 July happened later.

One row per opportunity.

## Columns

| column | what it is | who fills it |
|---|---|---|
| `deal_id` | CRM id, in order of creation | CRM |
| `company` | account name | AE |
| `techs` | number of field technicians, from LinkedIn / the prospect | AE |
| `state` | US state of the head office | AE |
| `incumbent` | the field-service tool they use today — **asked on the first call** | AE |
| `rep` | the AE who owns the opportunity | lead router |
| `created_date` | when the opportunity record was created | CRM, automatic |
| `first_meeting_date` | date of the first meeting that actually happened — blank if none was logged | AE |
| `calls` | calls logged against the opportunity | AE |
| `last_activity_date` | last logged call, email or note | CRM, automatic |
| `stage` | Discovery → Demo → Proposal → Closed won / Closed lost — **moved by hand** | AE |
| `close_date` | when it was set to Closed won or Closed lost | CRM, automatic |
| `amount_usd` | annual contract value at **list price**: `techs` × $275, rounded to the nearest $100 (an exact $50 rounds to the even hundred), minimum $9,600. Discounts and signed amounts are not recorded here | CRM, from `techs` |
| `lost_reason` | picked from a dropdown when closing as lost | AE |

## What sales ops told us

- **An AE creates the opportunity when they send the prospect a booking link.** That is the team's
  rule since the AI SDR went live: the AE reads each reply the classifier marked "interested", sends a
  booking link to the ones that look real, and the link gets an opportunity, so nothing falls through
  the cracks. Replies the AE judged not real get no link and no opportunity.
- **Interested replies are assigned by a lead router.** The sales manager sets the weights: Mike
  R. 45%, Jess Alvarez 28%, Owen Pratt 27%. Every AE gets leads from every state.
- **LinkedIn restricted the AI SDR's sending account on 25 June 2026** (the weekly invite cap), and
  no connection requests went out until 4 August. Anything created in July is a late reply to June's
  invites.
- **Nobody closes an opportunity for them.** An open one stays open until its AE changes the stage.
- The CRM holds no call recordings. The only recorded calls are Northline's, in
  `customers/northline-mechanical/calls/`.
