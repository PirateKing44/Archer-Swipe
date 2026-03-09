# Skill: LinkedIn Miner

## Purpose
Mine LinkedIn engagement (post comments, reactions) to find warm leads who are already engaging with relevant content in your space.

## When to Use
When your enrichment agent needs fresh leads that show buying signals — people actively commenting on or reacting to posts about topics relevant to your ICP.

## Required API Keys
```env
# Add to your .env file:
RAPIDAPI_KEY=your_key_here
```
**Service:** Fresh LinkedIn Profile Data (via RapidAPI)
**Pricing:** Check https://rapidapi.com/freshdata-freshdata-default/api/fresh-linkedin-profile-data/pricing
**Rate limits:** 200-500 posts/day recommended. Check `x-ratelimit-remaining` header.

## API Reference

**Host:** `fresh-linkedin-profile-data.p.rapidapi.com`

### Search Posts
```
POST /search-posts
Headers:
  x-rapidapi-key: {{RAPIDAPI_KEY}}
  x-rapidapi-host: fresh-linkedin-profile-data.p.rapidapi.com
Body:
  { "keyword": "{{YOUR_SEARCH_KEYWORD}}", "count": 20 }
```

### Get Post Comments
```
GET /get-post-comments?urn={{POST_URN}}&count=50
```

### Get Post Reactions
```
GET /get-post-reactions?urn={{POST_URN}}&count=50
```

### Get Profile Posts
```
GET /get-profile-posts?linkedin_url={{LINKEDIN_URL}}&count=10
```

## Data Flow
```
Search keyword → Find relevant posts → Extract commenters + reactors →
Deduplicate by LinkedIn URL → Rank (commenters > reactors) →
Output: output/YYYY-MM-DD/02-leads.json
```

## Output Format
```json
[
  {
    "name": "Jane Smith",
    "linkedin_url": "https://linkedin.com/in/janesmith",
    "title": "Managing Director",
    "company": "Acme Capital",
    "source_post_urn": "urn:li:activity:1234567890",
    "engagement_type": "comment",
    "engagement_text": "Great insights on...",
    "extracted_at": "2026-01-15T08:00:00Z"
  }
]
```

## Next Step
Feed output to → **lead-enricher** skill (finds emails for these leads)
