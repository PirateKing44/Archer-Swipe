# CRM Enrichment & Contact Discovery — Operational Swipe File

> Stripped from a real production system. Sensitive data (API keys, tokens, IDs) replaced with placeholders.

## Overview
Two workflows that run together on a weekly schedule:
- **Workflow A (Enrichment):** Fix broken, missing, or wrong LinkedIn URLs on existing CRM contacts
- **Workflow B (Discovery):** Find new target firms and populate 1-2 fresh contacts per firm into the CRM

## Schedule
Weekly — pick a consistent day and time (e.g., Fridays 6:00 AM)

## Trust Level
Conservative. HIGH-confidence enrichment writes are autonomous. All discovery
contacts require human sign-off before outreach touches them.

---

## WORKFLOW A — ENRICHMENT (Fix Existing Contacts)

### Trigger
Scheduled (weekly). Also on-demand when your outreach agent flags a contact
with [URL_ENRICHMENT_NEEDED] in the notes.

### Pass 1: Flag Audit

1. Pull all contacts from your CRM.
2. Build the enrichment queue — include a contact if ANY of these are true:
   - LinkedIn URL is null or empty
   - Notes contain [URL_ENRICHMENT_NEEDED] or [WRONG CONTACT]
   - LinkedIn URL slug doesn't match the contact's name (zero word overlap = flag it)
3. Log the queue count. If empty, skip to Workflow B.
4. Post to your Slack channel: "Starting enrichment run [date]. Flagged contacts: [X]"

### Pass 2: Enrich Each Contact

For each contact in the queue:

**Step 1 — Resolve company LinkedIn URL**
- Normalize the firm name to a slug: lowercase, replace spaces with hyphens, remove punctuation
  - Example: "Soros Fund Management" → `soros-fund-management`
- Construct URL: `https://www.linkedin.com/company/{slug}/`
- If the firm name is ambiguous → use a Google Search scraper to find the company LinkedIn URL first

**Step 2 — Scrape people from the company page**
Use a LinkedIn Company People Scraper (Apify, PhantomBuster, or similar):
- Input: Company LinkedIn URL + target positions (e.g., "CIO", "Business Development")
- Max items: 5-10 per company
- Use residential proxy
- LinkedIn cookies required (li_at + JSESSIONID from an active session)

Expected output per person: name, job title, LinkedIn profile URL, location

**Step 3 — Match and score confidence**

| Confidence | Criteria | Action |
|------------|----------|--------|
| HIGH | Name matches (case-insensitive, 2+ words match) AND title matches target role AND company matches firm | Auto-update CRM |
| MEDIUM | Name matches but title is ambiguous or lists firm as former employer | Flag for human review — do NOT auto-update |
| LOW | No name match, multiple equally plausible matches, or company URL couldn't be resolved | Create task for human — do NOT auto-update |

**Step 4 — Write to CRM (HIGH confidence only)**
Update the contact's LinkedIn URL. Append to notes: "Enriched [date]: replaced [old_url] with [new_url] (HIGH confidence)"

**Step 5 — Escalate MEDIUM and LOW**
Create a review task for each with: contact name, firm, current URL, candidate URL, candidate title, reason for non-HIGH confidence.

### Pass 3: Deduplication

1. Pull all contacts with their LinkedIn URLs and name+firm composite
2. Build two indexes:
   - URL index: map of linkedin_url → list of contact IDs
   - Name+firm index: map of lowercase(name)+lowercase(firm) → list of contact IDs
3. Flag any entry with more than one contact ID as a potential duplicate
4. Create a review task for each duplicate pair. Do NOT delete contacts — flag only.

---

## WORKFLOW B — DISCOVERY (Find New Target Contacts)

### Trigger
Runs after Workflow A on schedule day. NOT triggered on-demand runs.

### Cap
10 new contacts per run maximum. Quality over quantity.

### Steps

**Step 1 — Profile existing CRM firms**
- Pull all contacts, extract unique firm names
- Infer top firm types by keyword:
  - "Family Office/Capital/Wealth" → family office
  - "Wealth Management/Partners/Advisors" → wealth management
  - "Alternative/Alt Assets" → alternative asset manager
  - "Endowment/Foundation" → endowment/foundation
  - "RIA/Registered Investment" → RIA
  - "Fund of Funds/Multi-Strategy" → fund of funds
- Identify top 3 firm types by frequency — these drive discovery queries

**Step 2 — Find new firms via Google Search**
For each of the top 3 firm types, run a search query:
- `site:linkedin.com/company "[firm type]" "[target role]"`
- Extract LinkedIn company URLs from results

**Step 3 — Dedup firms**
- Normalize each company URL slug
- Check against known firms set from Step 1
- Check against "Previously Searched Firms" list (skip any searched in last 30 days)
- Cap discovery queue at 8 firms per run

**Step 4 — Scrape people from each new firm**
Same scraper as Workflow A. Target positions: your ICP roles.
Max 5 people per company.

**Step 5 — Select 1-2 contacts per firm**
Priority:
- Slot 1: First person whose title matches primary ICP role
- Slot 2: First person matching secondary ICP role
- If only one match → take it. Never force a weak second pick.
- If zero matches → skip this firm entirely. Log it.

**Step 6 — Dedup contacts against existing CRM**
Check both name+firm and LinkedIn URL before creating.

**Step 7 — Create new CRM contacts**
- Stage: "target" (not yet contacted)
- Source: "agent_discovery"
- Notes: "[DISCOVERED_BY_AGENT] [date] | Firm type: [type] | Role matched: [role]"
- Tag for human review before outreach agent touches them

### End-of-Run Report

Post to Slack:
```
Enrichment + Discovery run complete [date].

Enrichment:
- Contacts audited: [X]
- AUTO-fixed (HIGH confidence): [X]
- Flagged for review (MEDIUM): [X]
- Needs manual research (LOW): [X]
- Duplicates flagged: [X]

Discovery:
- Firms searched: [X]
- New contacts created: [X] (tagged for review)
- Firms with no role match: [X]

Action needed: [X] tasks await your review.
```

---

## ERROR HANDLING

- **Scraper auth failure (401/403):** LinkedIn cookies expired. Stop run immediately. Create task: "LinkedIn cookies expired — need fresh li_at + JSESSIONID." Do not retry.
- **Scraper timeout (>60 min):** Company page may be private or slug is wrong. Log LOW confidence, skip, continue.
- **Zero results for a company:** Try Google Search fallback for the specific person. If still nothing, log as LOW.
- **CRM update returns 404:** Contact ID is stale. Re-pull contacts, re-match by name+firm, retry.

---

## RULES (Hard Guardrails)

- NEVER auto-update CRM for anything below HIGH confidence
- NEVER delete contacts. Flag duplicates for human review only.
- ALWAYS dedup before creating any new contact
- ALWAYS log the confidence level and rationale for every enrichment action
- ALWAYS cap discovery at 10 contacts per run
- ALWAYS tag discovered contacts for human review before outreach

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
