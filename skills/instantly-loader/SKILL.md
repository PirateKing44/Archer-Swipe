# Skill: Instantly Loader

## Purpose
Push approved email sequences to Instantly.ai for automated sending. This is the FINAL step — the trigger that actually sends emails to real people.

## When to Use
After outreach-writer produces `06-emails.json` AND owner has:
1. Reviewed and approved email samples
2. Set `quality_check_passed: true` on the batch

**REQUIRES EXPLICIT OWNER APPROVAL BEFORE RUNNING.**

## Required API Keys
```env
# Add to your .env file:
INSTANTLY_API_KEY=your_key_here
```

## API Reference

**Base URL:** `https://api.instantly.ai/api/v1`

### List Campaigns
```
GET /campaign/list?api_key={{INSTANTLY_API_KEY}}
```

### Create Campaign
```
POST /campaign/create
Body:
  {
    "api_key": "{{INSTANTLY_API_KEY}}",
    "name": "{{CAMPAIGN_NAME}} — {{DATE}}"
  }
```

### Add Leads to Campaign
```
POST /lead/add
Body:
  {
    "api_key": "{{INSTANTLY_API_KEY}}",
    "campaign_id": "{{CAMPAIGN_ID}}",
    "leads": [
      {
        "email": "jane.smith@acmecapital.com",
        "first_name": "Jane",
        "last_name": "Smith",
        "company_name": "Acme Capital",
        "custom_variables": {
          "sequence_1_body": "...",
          "sequence_2_body": "...",
          "sequence_3_body": "..."
        }
      }
    ]
  }
```

### Activate Campaign
```
POST /campaign/activate
Body:
  {
    "api_key": "{{INSTANTLY_API_KEY}}",
    "campaign_id": "{{CAMPAIGN_ID}}"
  }
```

## Process (with mandatory gates)

```
1. Check that 06-emails.json has quality_check_passed: true
   └─ If false → STOP. Tell owner samples need review first.

2. Create campaign in Instantly
   └─ Name format: "{{CAMPAIGN_PREFIX}} — YYYY-MM-DD"

3. Load leads into campaign

4. POST REPORT TO SLACK:
   "Campaign '{{name}}' loaded with {{count}} leads.
    Ready to activate. Awaiting your go."

5. [GATE] WAIT FOR EXPLICIT "activate" COMMAND FROM OWNER
   └─ Do NOT auto-activate. Ever.

6. Activate campaign

7. Confirm in Slack: "Campaign activated. {{count}} leads. First sends begin within 1 hour."
```

## Daily Sending Limits
- **Recommended:** 50 sends per account per day
- Configure this in Instantly dashboard, not via API
- If you have multiple sending accounts, Instantly rotates automatically

## Safety Rules
1. **Never activate without explicit owner approval**
2. **Never load leads that haven't passed quality check**
3. **Never exceed daily send limits**
4. **Always report load count before activation**
5. **Log everything: campaign ID, lead count, activation time**
