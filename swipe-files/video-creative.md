# Video/Creative Production — Operational Swipe File

> Stripped from a real production system. Sensitive data (API keys, tokens, IDs) replaced with placeholders.

## Overview
Produce UGC-style images and short-form video ads at scale using a multi-provider
AI generation pipeline with a review hub (e.g., Airtable) as the approval system.

One linear pipeline. No branching. Multiple human checkpoints. Cost confirmation
required before every generation step.

## Trust Level
Conservative for all generation and delivery. Autonomous only for reading tasks
and checking review status.

---

## CREATIVE ENGINE PIPELINE

```
Brief → Brand Profile → Reference Analysis (optional) → Image Prompts →
Review Hub Records → [HUMAN REVIEW] → Image Generation → [HUMAN REVIEW] →
Video Prompts → Video Generation → [HUMAN REVIEW] → (optional) Extend → Deliver
```

### Step 0 — RECEIVE BRIEF

Accept a brief from your task queue or human direction. Extract:
- **Brand:** Which brand this content is for
- **Topic/Product:** What the content is about
- **Quantity:** How many ad variations
- **Reference material:** Any reference videos or images provided
- **Special requests:** Model preferences, style notes, duration

Load the brand creative profile AND brand reference docs before writing any prompts.
The creative profile covers generation defaults. The reference docs cover audience,
voice, and messaging framework.

### Step 1 — ANALYZE REFERENCES (Optional but Recommended)

If reference videos are provided, analyze them before writing prompts.
Extract: hook style, person description, setting, camera work, pacing, tone,
dialogue patterns, authenticity score, and prompt notes.

Show the analysis summary to the human before proceeding.

### Step 2 — WRITE IMAGE PROMPTS & CREATE REVIEW RECORDS

1. Upload reference images to hosting (if your tool requires hosted URLs)
2. Create review hub records (e.g., Airtable rows) with:
   - Sequential index, ad name, product, brand
   - Reference images, image prompt, image model, status: Pending
3. Every record MUST have: Index (unique), Ad Name, Product, Brand, Reference Images, Image Prompt, Image Model, Image Status
4. Tell the human to review prompts. Wait for confirmation.

### Step 3 — COST CONFIRMATION & IMAGE GENERATION

**HARD RULE: NEVER generate without showing cost and getting explicit confirmation.**

Show:
- Number of records x variations = total images
- Model and provider
- Per-unit cost and total cost

Generate after confirmation. Images go directly to review hub.
Tell human to review and mark as Approved or Rejected.

**DO NOT proceed to video generation until images are approved.**

### Step 4 — WRITE VIDEO PROMPTS

Only after image approval. Write video prompts for approved images.

**For dialogue-capable models:** Put dialogue in quotes within natural description.
Include "maintains eye contact with camera" and "fixed camera" for UGC style.
Keep dialogue under 150 characters, casual and conversational.

Tell human to review video prompts before generating.

### Step 5 — COST CONFIRMATION & VIDEO GENERATION

Same cost confirmation rule. Show breakdown, wait for explicit yes.
Generated videos go to review hub.

### Step 5.5 — EXTENDED VERSIONS (Optional)

If your video model supports extending approved videos:
1. Write an extend prompt describing what should happen next
2. Human reviews extend prompt
3. Show cost breakdown (typically ~25% of full generation cost)
4. Generate after approval

### Step 6 — DELIVER

Once human approves final assets:
1. Post delivery note to originating task
2. Update context file with delivery record
3. Approved assets ready for distribution agents

---

## SMART DEFAULTS (override only when explicitly asked)

| Setting | Default | Override When |
|---------|---------|---------------|
| Aspect ratio | 9:16 vertical | User specifies landscape or square |
| Image variations | 2 per record | User specifies 1 |
| Video variations | 2 per record | User specifies 1 |
| Resolution | 1K | User asks for higher quality |

---

## PROMPT GUIDELINES

### Image Prompts
- Start every prompt with aspect ratio: "9:16."
- Describe a realistic person holding/using the product or in a relevant setting
- Reference input images where applicable
- Include amateur camera keywords: "amateur iPhone photo," "casual selfie," "slightly uneven framing"
- Include skin realism: "natural skin texture with visible pores, not airbrushed"
- Apply brand profile constraints (character age, setting, tone)

### Video Prompts
- Put dialogue in quotes within the prompt text
- Describe action, camera, and mood in plain text
- Always include "maintains eye contact with camera" and "fixed camera"
- Keep dialogue under 150 characters, casual and conversational
- Match dialogue to brand profile tone

---

## REVIEW HUB SCHEMA (e.g., Airtable)

| Field | Type | Purpose |
|-------|------|---------|
| Index | Number | Sequential ID |
| Ad Name | Text | Descriptive identifier |
| Product | Text | Product/topic name |
| Brand | Select | Which brand |
| Reference Images | Attachment | Product photos |
| Image Prompt | Long Text | Generation prompt |
| Image Model | Select | Model name |
| Image Status | Select | Pending / Generated / Approved / Rejected |
| Generated Image 1 | Attachment | Variation 1 |
| Generated Image 2 | Attachment | Variation 2 |
| Video Prompt | Long Text | Motion/dialogue prompt |
| Video Model | Select | Model name |
| Video Status | Select | Pending / Generated / Approved / Rejected |
| Generated Video 1 | Attachment | Variation 1 |
| Generated Video 2 | Attachment | Variation 2 |

---

## RULES (Hard Guardrails)

### Creative Engine Rules
1. NEVER call any generation endpoint without showing exact cost breakdown and getting explicit confirmation. No exceptions.
2. Always create review records with prompts FIRST. Have human review. THEN generate.
3. All images must be generated and reviewed before proceeding to video.
4. Every record must have a Brand field set. No untagged creatives.
5. Do NOT batch cost confirmations. Confirm images and videos separately.

### Brand Voice Rules
1. Always load the brand creative profile before writing any prompts. Never freestyle.
2. Never use alarmist or fear-based framing in health-related content.
3. Never make explicit medical claims.

### Prompt Rules
1. Every image prompt starts with aspect ratio
2. No double quotes inside prompts (breaks JSON serialization)
3. Video dialogue under 150 characters
4. Always include "fixed camera" and "maintains eye contact" for UGC prompts

### Workflow Rules
1. Review hub is the sole review system for creative content
2. Check human approvals before starting next pipeline step
3. Always write handoff notes before ending a session
4. Log every completed batch before session end

### Escalation Triggers
- Generation quality unacceptable after 2 retries
- Brief is ambiguous
- Brand direction unclear for new content type
- Required reference image missing
- API persistent errors
- Batch cost exceeds $25

### Correction Loop
```
## CORRECTION — [Date]
Batch: [description]
Issue: [what was flagged]
Root cause: [what went wrong]
Fix applied: [what changed]
Rule added/updated: [rule ID]
```

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
