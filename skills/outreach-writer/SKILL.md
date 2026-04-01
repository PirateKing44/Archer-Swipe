# Skill: Outreach Writer

## Purpose
Write personalized cold email sequences for scored leads. Produces 3-email sequences tailored to each lead's engagement context.

## When to Use
After icp-scorer produces `05-scored.json` with A and B tier leads that have `send_safe: true`. **Only run when owner explicitly requests email outreach.**

## Required API Keys
```
None — this is pure writing. No external API needed.
```

## Email Sequence Structure

### Email 1 — Initial Outreach (Day 0)
- Subject line: short, specific, no clickbait
- Open with relevance hook (why you're reaching out to THEM specifically)
- 1-2 sentences of value/context
- Soft CTA (question, not demand)
- Signed as {{YOUR_NAME}}

### Email 2 — Follow-up (Day 3)
- Reply-chain style (no new subject line)
- Add new value angle or proof point
- Reference something specific about their company
- Slightly more direct CTA

### Email 3 — Break-up (Day 7)
- Final touch
- Short — 2-3 sentences max
- Clear close: "If the timing isn't right, no worries at all."
- Leave door open

## Personalization Angles

### For LinkedIn-mined leads:
Reference the specific post they engaged with:
> "Saw your comment on [author]'s post about [topic] — [your connection to that topic]."

### For ICP-search leads:
Reference company situation or growth signals:
> "Noticed [company] recently [growth signal] — we've helped similar firms with [value prop]."

## Brand Config (customize these)
```
outreach.sender_name: {{YOUR_NAME}}
outreach.sender_title: {{YOUR_TITLE}}
outreach.tone: {{OUTREACH_TONE}}  # e.g., "professional but warm, founder-to-founder"
outreach.pain_points:
  - {{PAIN_POINT_1}}
  - {{PAIN_POINT_2}}
outreach.proof_points:
  - {{PROOF_POINT_1}}
  - {{PROOF_POINT_2}}
brand.value_prop: {{VALUE_PROP}}
```

## Data Flow
```
Input: output/YYYY-MM-DD/05-scored.json (A+B tier, send_safe only)
  → Write 3-email sequence per lead
  → Output: output/YYYY-MM-DD/06-emails.json
```

## CRITICAL GATE
**Always show owner 2-3 email samples for approval BEFORE generating the full batch.**
Do not mass-produce emails without explicit sign-off on tone and approach.

## Output Format
```json
[
  {
    "lead_name": "Jane Smith",
    "lead_email": "jane.smith@acmecapital.com",
    "tier": "A",
    "quality_check_passed": false,
    "emails": [
      { "sequence": 1, "day": 0, "subject": "...", "body": "..." },
      { "sequence": 2, "day": 3, "subject": null, "body": "..." },
      { "sequence": 3, "day": 7, "subject": null, "body": "..." }
    ]
  }
]
```
Note: `quality_check_passed` starts as `false` — owner sets to `true` after review.

## Next Step
Feed output to → **instantly-loader** skill (pushes to Instantly.ai for sending)
