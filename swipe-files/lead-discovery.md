# Lead Discovery — GTM Pipeline Operational Swipe File

> Stripped from a real production system. Sensitive data (API keys, tokens, IDs) replaced with placeholders.

## Overview
Full pipeline: Mine LinkedIn engagement for warm leads → Enrich with verified
emails → Score against ICP → Hand off to outreach agent (or write personalized
email sequences when explicitly told to).

## Trust Level
Conservative. Report counts and costs before each step. Never write outreach
emails unless explicitly commanded. Never activate campaigns without approval.

---

## PIPELINE FLOW

```
Mine LinkedIn → Enrich Emails → Verify Emails → Score Against ICP → Handoff
                                                                    ↓ (explicit command only)
                                                              Write Outreach → Load to Campaign Tool
```

### Step 0: Brand Config Selection

Always select a brand/ICP config before starting. Never mix ICPs in a single run.

Brand config contains:
- ICP criteria (industry, company size, role/title, geography, other criteria)
- LinkedIn mining topics and keywords
- Outreach tone and pain points
- Scoring weights

### Step 1: Mine LinkedIn

Search LinkedIn for engagement signals:
- Search posts by keyword (topics from brand config)
- Get commenters and reactors on relevant posts
- Get posts from industry influencers and extract engagers
- Output: raw leads file with LinkedIn URLs

Report lead counts before proceeding.

### Step 2: Enrich with Email Finder

For each lead, find verified email:
- Primary: Email finder API (e.g., LeadMagic — input: first_name, last_name, domain)
- Fallback: Secondary API (e.g., Apollo)
- Cost: typically 1 credit per found email, FREE for not_found
- **Approval gate:** Report count + estimated credit cost before enriching batches >50 leads
- Output: enriched leads file with emails + company data

### Step 3: Verify Emails

Validate each email address:
- Input: email address
- Output: status (valid / valid_catch_all / catch_all / invalid / unknown)
- Cost: typically fraction of a credit per check. catch_all and unknown often free.
- Only `valid` and `valid_catch_all` are "send-safe"
- Output: verified leads file with email validity status

### Step 4: Score Against ICP

Score each lead using brand config weights:
- **A tier (75-100):** Perfect ICP match. Right role, right company, right industry.
- **B tier (50-74):** Strong match. 2 of 3 criteria met.
- **C tier (25-49):** Possible. 1 criterion met.
- **D tier (0-24):** Not a fit. Archive.

Output: scored leads file with tier assignments

### Step 5: Handoff Decision

**Default path — hand off to outreach agent:**
1. Post summary to Slack:
   "GTM run complete [date] — [brand]. A-tier: [X] | B-tier: [X] | Send-safe A+B: [X]"
2. Create dashboard task with scored leads file location
3. STOP HERE. Do NOT write emails unless explicitly told to.

**Direct outreach path (explicit command only):**
Proceed to Step 6.

### Step 6: Write Outreach (explicit command only)

- Filter to A and B tier leads with send_safe: true
- Write 3-email sequences per lead using brand config tone + pain points
- Personalize based on: their role, company, LinkedIn activity that triggered mining
- **Always show sample emails before loading to campaign tool. Wait for approval.**

### Step 7: Load to Campaign Tool (after explicit approval)

- Two approval gates: (1) human reviews email samples, (2) human confirms activation
- Never activate a campaign without explicit "yes"
- Daily send limits configured per campaign

---

## PIPELINE OUTPUT FILES

| Step | File | Contents |
|------|------|----------|
| Mining | `output/YYYY-MM-DD/02-leads.json` | Raw engagers from LinkedIn |
| Enrichment | `output/YYYY-MM-DD/03-enriched.json` | Leads with emails + company data |
| Verification | `output/YYYY-MM-DD/04-verified.json` | Leads with email validity status |
| Scoring | `output/YYYY-MM-DD/05-scored.json` | Tiered leads with ICP scores |
| Emails | `output/YYYY-MM-DD/06-emails.json` | 3-email sequences per lead |

---

## PRE-CALL RESEARCH (On-Demand)

When asked to prep for a sales call, produce a 1-page briefing doc:
- Company overview, recent news, key people
- Their likely pain points based on ICP analysis
- Connection points (shared connections, topics, interests)
- Suggested talking points and questions
- Deliver at least 30 minutes before the call

---

## RULES (Hard Guardrails)

- NEVER write outreach emails unless explicitly commanded
- NEVER activate campaigns without explicit human approval
- NEVER mix ICPs in a single pipeline run
- ALWAYS report lead counts and credit costs before enriching large batches
- ALWAYS show sample emails before loading to any campaign tool
- ALWAYS use brand config for ICP scoring — never freestyle criteria
- Two approval gates for campaign loading: email review + activation confirmation
- Cap contacts per discovery run (prevents quantity-over-quality drift)
- Never deliver unverified contacts to outreach
- Always log source for every contact (for attribution and pattern recognition)
- Deduplicate against existing CRM before delivering

---

## ERROR HANDLING

- **LinkedIn cookies expired (401/403):** Stop immediately. Create task. Do not retry.
- **API rate limit:** Back off, wait, retry once. If still failing, escalate.
- **Zero mining results:** Topic may be too niche. Try broader keywords. Log findings.
- **Low enrichment rate (<20% emails found):** Likely wrong domains or very private contacts. Report to human before continuing.

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
