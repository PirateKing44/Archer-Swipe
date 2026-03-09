# {{CHARACTER_NAME}} — Tools & Integrations

## Review Hub
<!-- CONFIGURE: Your creative asset tracking tool -->
- **Airtable Base:** Uses `AIRTABLE_API_KEY` + `AIRTABLE_BASE_ID` from .env
- Tables: Briefs, Image Prompts, Generated Assets, Video Assets
- Every asset gets a record with: brand, date, cost, status, approval

## Image Generation
<!-- CONFIGURE: Uncomment your tool -->

<!-- ### Google AI Studio
- Uses GOOGLE_API_KEY from .env
- Model: Gemini with image generation -->

<!-- ### Midjourney
- Via Discord bot or API
- Requires MIDJOURNEY_API_KEY -->

<!-- ### DALL-E / OpenAI
- Uses OPENAI_API_KEY from .env -->

<!-- ### Flux / Replicate
- Uses REPLICATE_API_KEY from .env -->

## Video Generation
<!-- CONFIGURE: Uncomment your tool -->

<!-- ### Kie AI (lip-sync)
- Uses KIE_API_KEY from .env -->

<!-- ### WaveSpeed
- Uses WAVESPEED_API_KEY from .env -->

<!-- ### Runway
- Uses RUNWAY_API_KEY from .env -->

## Slack
- Your channel: #{{CREATIVE_CHANNEL_NAME}}
- Post all review-ready assets here
- Receive briefs and scripts here

## File System
- Briefs: `workspace/briefs/`
- Brand profiles: `workspace/brand-profiles/`
- Output: `workspace/output/`
