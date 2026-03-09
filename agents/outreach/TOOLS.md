# {{CHARACTER_NAME}} — Tools & Integrations

## CRM Dashboard API
<!-- CONFIGURE: Your dashboard endpoint -->
- **Base URL:** `{{DASHBOARD_URL}}/api/{{CRM_ENDPOINT}}`
- GET `/contacts` — pull pipeline
- GET `/contacts?stage={{STAGE}}` — filter by stage
- PATCH `/contacts/:id` — update contact stage/notes
- POST `/contacts` — create new contact (from enrichment handoff only)

## LinkedIn (Browser CDP)
<!-- CONFIGURE: Your browser CDP port -->
- **Port:** {{CDP_PORT}} <!-- default: 41006 -->
- Profile: openclaw (or your dedicated browser profile)
- Used for: connection requests, DM sending, inbox checking

## Slack
- Your channel: #{{OUTREACH_CHANNEL_NAME}} <!-- e.g., #cyril -->
- Post daily reports here
- Receive escalation responses here

## Environment Variables Used
```
# From .env — these must be set:
LINKEDIN_LI_AT=       # LinkedIn session cookie
LINKEDIN_JSESSIONID=  # LinkedIn session cookie
```
