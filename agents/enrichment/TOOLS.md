# {{CHARACTER_NAME}} — Tools & Integrations

## CRM Dashboard API
- **Base URL:** `{{DASHBOARD_URL}}/api/{{CRM_ENDPOINT}}`
- GET contacts for audit
- PATCH to update enriched data
- POST to create discovered contacts

## LeadMagic (Primary Email Enrichment)
- **API Key:** Uses `LEADMAGIC_API_KEY` from .env
- **Endpoints:**
  - Email finder: 1 credit per found email (free if not found)
  - Email validator: 0.05 credits per valid check
  - Status codes: `valid`, `valid_catch_all` (safe), `catch_all` (risky), `invalid` (remove), `unknown` (retry)

## Apollo.io (Fallback)
- **API Key:** Uses `APOLLO_API_KEY` from .env
- Free tier: 50 credits/month
- Use only when LeadMagic returns no result

## RapidAPI — Fresh LinkedIn Profile Data (LinkedIn Mining)
- **API Key:** Uses `RAPIDAPI_KEY` from .env
- **Host:** `fresh-linkedin-profile-data.p.rapidapi.com`
- **Endpoints:**
  - `POST /search-posts` — search by keyword
  - `GET /get-post-comments?urn={POST_URN}` — get commenters
  - `GET /get-post-reactions?urn={POST_URN}` — get reactors
  - `GET /get-profile-posts?linkedin_url={URL}` — get someone's posts
- **Rate limits:** 200-500 posts/day recommended. Check `x-ratelimit-remaining` header.
- Commenters rank higher than reactors. Deduplicate by LinkedIn URL.

## Apify (LinkedIn Profile Scraping)
- **Cookies Required:** `LINKEDIN_LI_AT` + `LINKEDIN_JSESSIONID` from .env
- Used for: fixing broken LinkedIn URLs, profile verification

## Slack
- Your channel: #{{ENRICHMENT_CHANNEL_NAME}}
- Outreach agent's channel: #{{OUTREACH_CHANNEL_NAME}} (for handoffs)

## File System — Pipeline Output
All pipeline runs save to `workspace/output/YYYY-MM-DD/`:
```
01-search.json      ← raw search results
02-leads.json       ← extracted leads (from linkedin-miner)
03-enriched.json    ← with emails (from lead-enricher)
04-verified.json    ← deliverability checked (from email-verifier)
05-scored.json      ← ICP scored A/B/C/D (from icp-scorer)
06-emails.json      ← draft sequences (from outreach-writer, if used)
```
