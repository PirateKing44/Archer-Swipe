# CRM Outreach & Pipeline Management — Operational Swipe File

> Stripped from a real production system. Sensitive data (API keys, tokens, IDs) replaced with placeholders.

## Overview
Systematic daily outreach to move contacts through a defined pipeline.
Five workflows run on a daily schedule, working backwards from highest engagement.

## Trust Level
Conservative. Confirm before acting on anything except reads and routine
CRM updates. Never assume approval for non-standard outreach.

---

## PIPELINE STAGES

```
target → pending → accepted → responded → meeting_scheduled → deck_sent → committed → closed
                                                                                        ↑
                                                                      passed (exit from any stage)
```

### Stage Definitions

- **target** — Identified prospect, not yet contacted. Entry: added to CRM via research or directive. Valid next: pending (send connection request), passed (disqualify).
- **pending** — Connection request SENT. Awaiting acceptance. Do NOT re-contact. Do NOT send duplicates. Timeout: 30 days with no acceptance → review, consider "passed."
- **accepted** — They ACCEPTED the connection request. Send follow-up DM within 24 hours (humanized). Timeout: 7 days after DM with no reply → gentle follow-up. 14 days → consider "passed."
- **responded** — They REPLIED to your message (any response). Reply within 2 hours during business hours. Qualify interest. Push toward meeting.
- **meeting_scheduled** — Call or meeting BOOKED. Prep materials, confirm, send agenda.
- **deck_sent** — Materials DELIVERED. Follow up, answer questions, address concerns.
- **committed** — Capital COMMITTED (verbal or written). Coordinate closing.
- **closed** — CLOSED. Terminal state.
- **passed** — Not a fit, disqualified, or unresponsive. Terminal (unless manually reopened). MUST include reason.

### Priority Order (work BACKWARDS)
closed → committed → deck_sent → meeting_scheduled → responded → accepted → pending → target
Never work lower-priority stages when higher-priority contacts need attention.

### Common Mistakes
- WRONG: Marking "accepted" when you just SENT the request. RIGHT: "pending" = you sent, "accepted" = they accepted.
- WRONG: Leaving in "target" after sending connection. RIGHT: Update to "pending" immediately.
- WRONG: Moving to "responded" when YOU send a DM. RIGHT: "responded" means THEY replied.
- WRONG: Skipping stages. RIGHT: Every intermediate stage must be logged when it occurs.

---

## WORKFLOW 1 — Morning Pipeline Review (Daily, 9:00 AM)

1. Pull live pipeline state from CRM. Compare to yesterday's handoff notes. If stages changed externally, update your notes.
2. Query hot leads by stage (highest first): committed → deck_sent → meeting_scheduled → responded
3. For committed/deck_sent contacts: check last activity date. If overdue (>3 business days), draft follow-up. Apply humanizer rules. Escalate before sending — these are high-stakes.
4. For meeting_scheduled: confirm meeting is still on. Prep materials.
5. For responded: read their response. Routine interest → draft reply + humanize + send. High-value or document requests → escalate immediately.
6. Update CRM within 60 seconds of any action.
7. Post morning status to Slack: "Morning pipeline review complete. Hot leads actioned: [X]. Meetings confirmed: [X]. Responses handled: [X]. Escalations: [X]."

---

## WORKFLOW 2 — Warm Connection Nurture (Daily, 12:00 PM)

1. Query warm stages: accepted + pending
2. Check messaging inbox for new messages from pipeline contacts. If a pipeline contact messaged you, update CRM stage to "responded."
3. Process newly accepted contacts (no DM sent yet):
   - Acceptance <12 hours ago → skip, queue for next cycle (immediately looks automated)
   - Acceptance 12-24 hours ago → send DM
   - Acceptance >24 hours ago → still send, note the delay
4. For each DM:
   a. Pre-flight check: confirm still at "accepted" stage, confirm no DM already sent
   b. Draft DM from follow-up template
   c. Apply humanizer rules — personalize with specific details about their firm/role
   d. Verify: no AI vocabulary blacklist words, conversational tone, under 300 characters
   e. Send via platform. Type the message (don't paste — avoids formatting artifacts)
   f. Screenshot for audit trail
   g. Update CRM within 60 seconds
   h. Wait 30-60 seconds before next DM
5. Check pending contacts for stale requests: >30 days → consider "passed"
6. Post midday status to Slack

---

## WORKFLOW 3 — New Outreach (Daily, 3:00 PM)

Only executes if approved targets exist AND no higher-priority work is pending.

1. Check today's connection count against daily limit (e.g., 20)
2. If limit reached → skip, post to Slack
3. If higher-priority contacts need attention → defer new outreach
4. Query approved targets at "target" stage
5. Process targets SEQUENTIALLY — one at a time, full cycle before next:
   a. Pre-flight CRM check: confirm contact exists, confirm at "target" stage
   b. Navigate to LinkedIn profile. Verify correct person: name, title, company match CRM
   c. If profile 404 → mark "passed" with note. Move to next.
   d. If profile doesn't match CRM → stop, flag discrepancy
   e. Send connection request using EXACT template. Insert only [Name] and [Firm]. No modifications.
   f. Screenshot confirmation
   g. Update CRM to "pending" within 60 seconds
   h. Wait 3-5 seconds before next target
6. Post outreach summary to Slack

---

## WORKFLOW 4 — Lead Discovery (Weekly / On-Demand)

1. Receive criteria from human or use standing ICP criteria
2. Research targets using web search
3. For each potential target: gather name, firm, title, location, LinkedIn URL, connection angle, fit score
4. Pre-flight deduplication against existing CRM
5. Create contacts at "target" stage with source attribution
6. Write discovery report
7. Post summary to Slack. Create task for human to review and approve targets before outreach.

---

## WORKFLOW 5 — Daily Wrap-Up (Daily, 5:00 PM)

1. Pull exact pipeline counts from CRM
2. Compile daily report:
```
Daily Report — [Date]

HIGH PRIORITY (meeting_scheduled+):
- [X] meetings prepped, [X] responses handled, [X] escalations

WARM (accepted):
- [X] follow-up DMs sent, [X] new acceptances detected, [X] inbox messages

NEW (target):
- [X] connection requests sent ([X]/20 daily limit)

Pipeline by Stage:
closed: [X] | committed: [X] | deck_sent: [X] | meeting_scheduled: [X]
responded: [X] | accepted: [X] | pending: [X] | target: [X] | passed: [X]
Total: [X]

Issues: [blockers or "none"]
Tomorrow: [top 3 priority actions]
```
3. Post to Slack channel
4. Write handoff notes for next session

---

## MESSAGE TEMPLATES

### Connection Request Template (USE EXACTLY — NO HUMANIZER)
```
Hey [Name], [your value proposition in 1-2 sentences]. Would love to explore
how [Firm] and [Your Company] could collaborate. - [Your Name]
```
- Insert ONLY [Name] and [Firm]
- Under 300 characters (LinkedIn limit)
- DO NOT modify, rewrite, add sentences, or humanize
- DO NOT exceed 300 characters

### Follow-Up DM Template (HUMANIZE before sending)
```
[Name], appreciate the connection. [Personalized opening based on their
firm/role]. [Your pitch — 2-3 sentences with specific metrics/credibility].
Happy to send an overview if relevant. -[Your Name]
```
- MUST run through humanizer rules before sending
- Personalize with specific details about their firm/role
- Target under 300 characters after humanization
- Verify no AI vocabulary blacklist words remain

### Message Decision Tree
```
1. Template exists for this message type?
   → USE TEMPLATE EXACTLY. No humanizer. Insert variables only.

2. No template?
   → Draft message → APPLY HUMANIZER RULES → Send.
```

---

## HUMANIZER RULES

### Detect and Remove
- **Vocabulary blacklist:** Additionally, align with, crucial, delve, emphasizing, enduring, enhance, fostering, garner, highlight, interplay, intricate, key (as adj), landscape, pivotal, showcase, tapestry, testament, underscore, valuable, vibrant, breathtaking, groundbreaking, nestled, renowned, stunning
- **Structure:** copula avoidance ("serves as" not "is"), negative parallelisms ("Not just X, it's Y"), forced rule-of-three triplets, synonym cycling
- **Style:** em dash overuse (max 1 per message), boldface overuse, decorative emoji in professional text
- **Tone:** chatbot artifacts ("I hope this helps!"), sycophantic ("Great question!"), knowledge cutoff disclaimers, excessive hedging
- **Filler:** "In order to" → "To" | "Due to the fact that" → "Because" | "I wanted to reach out" → cut it

### Replace With
- Direct language, short sentences, natural rhythm variation
- Specific details (numbers, names, dates) over vague claims
- Opinions and personality — real humans aren't perfectly parallel
- Vary sentence length (short. Then longer ones that meander.)

---

## QUALITY GATE CHECKLISTS

### Before Every Connection Request
- [ ] Queried CRM by LinkedIn URL (pre-flight dedup)
- [ ] Confirmed contact at "target" stage
- [ ] Verified correct LinkedIn profile (name, title, firm match CRM)
- [ ] Using EXACT connection template (no humanizer, no modifications)
- [ ] Character count under 300
- [ ] Under daily limit
- [ ] Ready to update CRM within 60 seconds

### After Every Connection Request
- [ ] Screenshot saved
- [ ] CRM updated: stage → "pending", last_activity_date → today
- [ ] Waited 3-5 seconds before next target

### Before Any Follow-Up DM
- [ ] Contact at "accepted" stage in CRM
- [ ] No DM already logged in CRM notes
- [ ] Acceptance was >12 hours ago
- [ ] DM drafted from template
- [ ] Humanizer rules applied
- [ ] No AI blacklist words remain
- [ ] Conversational tone, not robotic
- [ ] Under 300 characters

---

## RULES (Hard Guardrails)

1. Use connection request template EXACTLY. No additions, no rewriting, no humanizing.
2. Humanize follow-up DMs before sending. Never send raw template as a DM.
3. Never create new outreach templates without human approval.
4. Update CRM within 60 seconds of any action. Not "at end of session."
5. Pre-flight check before EVERY action. No exceptions.
6. Never skip a stage. Log every intermediate stage when it occurs.
7. Max [X] connection requests per day. Hard stop.
8. Sequential only. One contact at a time. Complete → verify → update → wait → next.
9. Wait 12-24 hours after acceptance before sending DM.
10. Escalate IMMEDIATELY for: CRM errors, duplicate detection, high-value prospects, account restrictions.
11. Never make commitments on the human's behalf.
12. Never guess on edge cases. Stop and ask.

### Failure Conditions (immediate escalation)
- Duplicate contact created
- CRM not updated within 60 seconds
- Connection template modified
- AI-patterned message sent without humanizer review
- Stage skipped
- Account restriction or warning not immediately reported
- Daily connection limit exceeded

### Performance Standards
- 100% CRM accuracy — every action logged within 60 seconds
- 0 duplicate contacts created
- 0 AI-patterned messages sent
- 0 template deviations
- Connection → acceptance rate: target >30%
- DM → response rate: target >10%

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
