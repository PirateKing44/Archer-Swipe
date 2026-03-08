# Skill: Email Verifier

## Purpose
Validate email deliverability before any outreach campaign. Removes invalid emails, flags risky ones, confirms safe-to-send addresses.

## When to Use
After lead-enricher produces `03-enriched.json`. This skill ensures you only email addresses that will actually land.

## Required API Keys
```env
# Add to your .env file:
LEADMAGIC_API_KEY=your_key_here
```

## API Reference

### LeadMagic — Email Validation
```
POST https://api.leadmagic.io/email-validate
Headers:
  x-api-key: {{LEADMAGIC_API_KEY}}
Body:
  { "email": "jane.smith@acmecapital.com" }
```

**Cost:**
- `valid` result: 0.05 credits (20 validations per credit)
- `catch_all` result: FREE
- `unknown` result: FREE
- `invalid` result: 0.05 credits

## Status Codes & Actions
| Status | Meaning | Action |
|--------|---------|--------|
| `valid` | Confirmed deliverable | Safe to email |
| `valid_catch_all` | Server accepts all (can't verify individual) | Safe to email (with flag) |
| `catch_all` | Risky — may bounce | Include with `risky: true` flag |
| `invalid` | Will bounce | **Remove from list** |
| `unknown` | Couldn't determine | Hold for retry next run |

## Data Flow
```
Input: output/YYYY-MM-DD/03-enriched.json (from lead-enricher)
  → Validate each email via LeadMagic
  → Tag with status
  → Remove invalid
  → Output: output/YYYY-MM-DD/04-verified.json
```

## Output Format
```json
[
  {
    "name": "Jane Smith",
    "email": "jane.smith@acmecapital.com",
    "email_status": "valid",
    "send_safe": true,
    "verified_at": "2026-01-15T10:00:00Z"
  }
]
```

## Next Step
Feed output to → **icp-scorer** skill (scores and tiers the leads)
