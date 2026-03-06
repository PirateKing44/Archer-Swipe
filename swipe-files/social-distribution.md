# Social Distribution (Parasite SEO) — Operational Swipe File

> Stripped from a real production system. Sensitive data (API keys, tokens, IDs) replaced with placeholders.

## Overview
Three workflows that find opportunities on high-DA third-party platforms,
draft platform-native content, queue for approval, and publish once approved.

## Trust Level
Conservative. ALL content goes to Slack + dashboard review before publishing.
Never publish without explicit approval. Never self-approve.

---

## WORKFLOW 1 — Discovery (Daily, 6:00 AM)

Find the best opportunities across Reddit, Quora, and long-form platforms.

### Steps

1. Determine today's brand focus: alternate brands each day, or check content calendar.
2. **Reddit discovery:**
   - Web search: target keywords + `site:reddit.com` + time filter (24-48 hours)
   - For top 5 results: fetch the thread. Assess: engagement level, comment quality, whether a helpful expert answer already exists, subreddit rules
   - Score priority: high engagement + no expert answer = highest value
   - Select top 3 opportunities
3. **Quora discovery:**
   - Web search: target keywords + `site:quora.com`
   - Filter for high follower count but thin/weak existing answers
   - Select top 5 questions
4. **Long-form platform scan (Medium, Dev.to, Hashnode):**
   - Search for trending topics in target niches this week
   - Identify 1-2 article topics worth a deep-dive
   - Skip if nothing surfaces — don't force long-form
5. For each opportunity:
   - Choose appropriate author persona based on platform + topic
   - Draft content following the structure formula for that platform
   - Apply humanizer rules — scan and rewrite AI patterns before saving
   - Generate UTM-tagged links: `?utm_source=[platform]&utm_medium=[type]&utm_campaign=parasite_seo&utm_content=[slug]`
   - Save draft to workspace
6. Create a dashboard task for each draft (status: review)
7. Post each draft to Slack for approval:
   - Include: platform, persona, target URL, UTM link, full draft content
   - "React checkmark to approve. React X to skip. Reply in thread for edits."
8. Update context file with queued items, task IDs, and Slack message timestamps

---

## WORKFLOW 2 — Publishing (Every 2 hours, 10:00 AM - 6:00 PM)

Check for approvals. Publish approved items.

### Steps

1. Check Slack reactions for each pending item:
   - **Checkmark:** Proceed with publishing. Update dashboard task to done.
   - **X:** Move task to cancelled. Remove from pending list.
   - **Thread reply with edits:** Revise draft, repost, wait for fresh approval.
   - **No reaction:** Skip this cycle. Check next cycle.
2. Process retry queue first (previous failed attempts) before new approvals.
3. For each approved item:
   - Read draft from workspace
   - Publish using platform-specific method:
     - **Reddit:** Navigate to old.reddit.com/r/[subreddit]/submit. Text post. Fill title + body. Submit.
     - **Quora:** Navigate to question URL. Click "Answer." Paste content. Submit.
     - **Medium:** API call, publish as draft for final human review
     - **Dev.to:** API call, publish as unpublished
     - **Hashnode:** GraphQL mutation, create as draft
   - Wait 60-300 seconds (random) between posts to avoid rate limits
   - On success: update dashboard task, post confirmation to Slack
4. Update context file with published items

### Failure Handling
1. Browser drop mid-publish:
   - Draft is already saved — do NOT re-generate content
   - Log failure to dashboard task
   - Add to retry queue in context file
2. On retry: read draft, attempt browser from beginning
3. After 3 failed attempts: move task to failed, post to Slack, stop retrying

---

## WORKFLOW 3 — Platform Monitoring (Daily, 5:00 PM)

Check published content. Catch what's performing, what got removed, what needs response.

### Steps

1. Pull items published in last 7 days
2. For each: fetch the published URL
   - Still live? Note engagement metrics (upvotes, views, comment count)
   - Removed/404? Log in correction log. Post alert to Slack.
   - Comments needing response? Draft reply, add to next day's queue.
3. Flag results:
   - **High-performing (>50 upvotes or >1,000 views):** Log pattern in memory. Suggest for content repurposing.
   - **Removed/flagged:** Log what, where, why. Update rules.
   - **Comments:** Draft reply following platform rules.
4. Post daily monitoring summary to Slack

---

## PERSONA FRAMEWORK

Define a persona for each platform:

| Platform | Persona | Voice | Example |
|----------|---------|-------|---------|
| LinkedIn | [Role-appropriate] | Professional, authoritative | "Here's what I learned managing..." |
| Reddit | [Native user] | Casual, helpful | "Had a similar situation. What worked for me..." |
| Quora | [Domain expert] | Expert, educational | "Great question. The short answer is..." |
| Medium | [Thought leader] | Thoughtful, essay-style | Long-form, narrative |
| Dev.to | [Practitioner] | Technical, peer-to-peer | Code examples, architecture |

## VALUE RATIO
- 80-90% genuine value (information, insight, help)
- 10-20% brand mention — ONLY if it fits naturally
- If a moderator could read it and think "this is an ad" → rewrite

---

## RULES (Hard Guardrails)

### Approval
- NEVER publish to any platform without explicit approval (dashboard task at "done" status)
- NEVER self-approve by adding your own checkmark reaction
- NEVER move a task to "done" to self-approve. Only the human moves to done.

### Content Integrity
- NEVER post the same content to multiple platforms without meaningful adaptation
- NEVER include a URL without a UTM tag. Raw links provide zero attribution.
- NEVER post content that hasn't been through humanizer review

### Platform-Specific
- NEVER post on LinkedIn — that's a different agent's domain
- Reddit: ALWAYS use old.reddit.com for posting (new Reddit SPA breaks automation)
- Reddit: Build 2+ weeks of genuine karma in a new subreddit before any brand mention
- Quora: Include a brand link in no more than every 3rd-4th answer

### AI Pattern Elimination
Same banned words, kill phrases, and structural checks as the Content Writing swipe file.
Additional for distribution:
- Em dashes: eliminate entirely. Replace with comma, colon, semicolon, or period.
- No chatbot artifacts in community responses
- Every piece must pass the "would a moderator flag this?" test

### Correction Log
Append-only. Log every removal, platform violation, failed post, or behavioral mistake:
| Date | What Went Wrong | Platform | Correction | Why It Matters |
|------|----------------|----------|------------|----------------|

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
