# Skill: ICP Scorer

## Purpose
Score and tier leads (A/B/C/D) against your Ideal Customer Profile. Only A and B tier leads get handed to outreach.

## When to Use
After email-verifier produces `04-verified.json`. This skill determines which leads are actually worth contacting.

## Required API Keys
```
None — this is pure logic. No external API needed.
```

## Scoring Dimensions (100 points total)

Customize these weights and criteria for your business:

| Dimension | Points | How to Score |
|-----------|--------|-------------|
| Title match | 25 | Exact ICP title = 25, adjacent = 15, unrelated = 0 |
| Industry match | 20 | Target industry = 20, adjacent = 10, unrelated = 0 |
| Company size | 20 | Above threshold = 20, close = 10, below = 0 |
| Seniority level | 15 | C-suite = 15, VP = 12, Director = 8, Manager = 4 |
| Engagement quality | 10 | Commented = 10, reacted = 5, no engagement = 0 |
| Growth signals | 10 | Hiring/funding/expansion = 10, stable = 5, declining = 0 |

## Tier Definitions
| Tier | Score | Action |
|------|-------|--------|
| **A** | 80-100 | Perfect match. Hand to outreach immediately. |
| **B** | 60-79 | Strong match. Include in handoff. |
| **C** | 40-59 | Partial match. Hold for review. Do NOT auto-handoff. |
| **D** | 0-39 | Poor match. Archive. Do not contact. |

## ICP Configuration
Customize in the enrichment agent's USER.md:
```
Target titles: {{TARGET_TITLES}}
Target industries: {{TARGET_INDUSTRIES}}
Min company size: {{MIN_COMPANY_SIZE}}
Geography: {{TARGET_GEO}}
Key signals: {{KEY_SIGNALS}}
```

## Data Flow
```
Input: output/YYYY-MM-DD/04-verified.json (from email-verifier)
  → Score each lead against ICP dimensions
  → Assign tier (A/B/C/D)
  → Output: output/YYYY-MM-DD/05-scored.json
```

## Output Format
```json
[
  {
    "name": "Jane Smith",
    "email": "jane.smith@acmecapital.com",
    "send_safe": true,
    "icp_score": 85,
    "tier": "A",
    "score_breakdown": {
      "title_match": 25,
      "industry_match": 20,
      "company_size": 20,
      "seniority": 12,
      "engagement": 5,
      "growth_signals": 3
    }
  }
]
```

## Handoff Rules
- **Default:** Hand A+B leads to outreach agent (via Slack + CRM)
- Do NOT hand off directly to outreach-writer or instantly-loader unless owner explicitly says to
- C and D tier leads never get handed off automatically

## Next Step (if using email campaigns)
Feed output to → **outreach-writer** skill (writes personalized email sequences)
