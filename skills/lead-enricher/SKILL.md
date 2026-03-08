# Skill: Lead Enricher

## Purpose
Find verified work emails for leads extracted by the LinkedIn Miner. Uses LeadMagic as primary service with Apollo.io as fallback.

## When to Use
After linkedin-miner produces `02-leads.json`. This skill adds email addresses so leads become contactable.

## Required API Keys
```env
# Add to your .env file:
LEADMAGIC_API_KEY=your_key_here     # Required (primary)
APOLLO_API_KEY=your_key_here        # Optional (fallback, 50 free credits/month)
```

## API Reference

### LeadMagic — Email Finder (Primary)
```
POST https://api.leadmagic.io/email-finder
Headers:
  x-api-key: {{LEADMAGIC_API_KEY}}
Body:
  {
    "first_name": "Jane",
    "last_name": "Smith",
    "company_domain": "acmecapital.com"
  }
```
**Cost:** 1 credit per email found. FREE if no email found.

### Apollo.io — Email Finder (Fallback)
```
POST https://api.apollo.io/v1/people/match
Headers:
  x-api-key: {{APOLLO_API_KEY}}
Body:
  {
    "linkedin_url": "https://linkedin.com/in/janesmith"
  }
```
**Cost:** Free tier = 50 credits/month.

## Data Flow
```
Input: output/YYYY-MM-DD/02-leads.json (from linkedin-miner)
  → Try LeadMagic first
  → If no result, try Apollo fallback
  → Output: output/YYYY-MM-DD/03-enriched.json
```

## Output Format
```json
[
  {
    "name": "Jane Smith",
    "linkedin_url": "https://linkedin.com/in/janesmith",
    "title": "Managing Director",
    "company": "Acme Capital",
    "email": "jane.smith@acmecapital.com",
    "email_source": "leadmagic",
    "email_confidence": "high",
    "enriched_at": "2026-01-15T09:00:00Z"
  }
]
```

## Next Step
Feed output to → **email-verifier** skill (validates deliverability)
