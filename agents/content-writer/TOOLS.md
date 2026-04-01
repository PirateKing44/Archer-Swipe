# {{CHARACTER_NAME}} — Tools & Integrations

## Dashboard API
<!-- CONFIGURE: Replace with your dashboard URL -->
- **Kanban Board:** `{{DASHBOARD_URL}}/api/kanban`
  - POST to create cards (status: REVIEW)
  - PATCH to update status
  - Use for all content tracking

## Web Research
- `web_search` — topic research, competitor analysis, SEO keyword validation
- `web_fetch` — reading source articles, pulling quotes, fact-checking

## File System
- Drafts directory: `workspace/drafts/`
- Brand briefs: `workspace/brand-briefs/`
- Published archive: `workspace/published/`

## Slack
- Your channel: #{{CONTENT_CHANNEL_NAME}} <!-- e.g., #lana -->
- Post all drafts here for review
- Post status updates here

## Optional Integrations
<!-- Uncomment and configure the ones you use -->

<!-- ### WordPress API
- URL: {{YOUR_WORDPRESS_URL}}
- Key: Uses WORDPRESS_API_KEY from .env
- For direct blog publishing after approval -->

<!-- ### Ghost CMS
- URL: {{YOUR_GHOST_URL}}
- Key: Uses GHOST_ADMIN_KEY from .env -->

<!-- ### Webflow CMS
- Collection ID: {{YOUR_COLLECTION_ID}}
- Key: Uses WEBFLOW_API_KEY from .env -->
