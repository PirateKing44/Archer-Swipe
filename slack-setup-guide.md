# Slack Setup Guide for Your AI Agent Team

## Why Slack?

Slack is the communication layer between you and your AI agents. Each agent gets their own channel. You review work, approve content, and manage your team through emoji reactions and thread replies — the same way you'd manage human employees.

Why not email? Why not a dashboard alone? Because Slack gives you:
- **Async review** — approve a content draft at 11 PM from your phone
- **Emoji workflows** — one tap to approve, reject, or request revisions
- **Thread context** — feedback stays attached to the work it references
- **Channel separation** — each agent's work is organized, not mixed together

---

## Channel Setup

### Naming Convention

All lowercase. Character first name only. No role names in channel names.

**Why:** You want to manage a team, not a tool list. `#lana` is a person. `#content-writer` is a machine. The character-based naming reinforces the agent-as-teammate model and makes Slack feel alive.

### Create Your Channels

For each agent the Character Generator created, make a channel:

```
#[character-first-name] — [Role] — [What gets posted here]
```

**Example (Archer-themed team):**
```
#lana      — Content Writing — drafts, revisions, weekly content reports
#cyril     — CRM Outreach — daily pipeline reports, connection summaries, escalations
#pam       — Social Distribution — platform drafts, publishing confirmations, monitoring alerts
#krieger   — Video/Creative — generation briefs, cost confirmations, asset deliveries
#framboise — CRM Enrichment — enrichment reports, discovery summaries, duplicate flags
```

### Optional Channels

```
#hq        — Command center. Cross-agent coordination, escalations, system-wide announcements.
#standup   — Daily standup summaries from all agents (if you set up a morning rollup).
```

---

## Approval Workflow

This is the core of how you manage your AI team. Every agent posts work for your review. You respond with emoji reactions.

### Reaction Guide

| Emoji | Meaning | What Happens |
|-------|---------|-------------|
| :white_check_mark: (checkmark) | **Approve** | Agent proceeds — publishes, sends, or moves to next step |
| :x: (X) | **Reject** | Agent stops. Read the thread for your feedback, then revise. |
| :arrows_counterclockwise: (loop) | **Revise** | Agent reads your thread reply for specific revision notes, updates, and reposts |
| :eyes: (eyes) | **Seen / Reviewing** | Agent knows you saw it but waits for a final decision |
| :double_vertical_bar: (pause) | **Hold** | Agent parks the work. Won't proceed until you remove the hold. |

### How It Works in Practice

1. **Agent posts a draft** to their channel (e.g., `#lana` posts a blog draft)
2. **You read it** — on your phone, laptop, whenever
3. **You react:**
   - Checkmark → agent moves the piece to "done" and hands it to the next agent in the pipeline
   - X → agent reads your thread reply, revises, reposts
   - Loop → agent makes specific changes you noted in the thread
4. **Nothing happens until you react.** Agents never self-approve. Silence is not consent.

### Thread Replies for Feedback

When you react with X or loop, **always leave a thread reply** explaining what needs to change. Agents will read the thread and apply your feedback.

Example thread reply:
> "Too formal. Make it sound more like a real person talking. Also, the CTA feels forced — soften it or remove it entirely."

---

## Agent Posting Format

Train your agents to post in a consistent format so everything is scannable:

### Content Drafts
```
DRAFT — TASK-[id]
Type: [Blog Post / Quora Answer / Social Copy / Video Script]
Brand: [Your Brand Name]
Target keyword: [keyword]
Length: ~X words

[First 300 words of draft...]

[Full draft in thread or attached file]

Notes: [Any flags or questions for you]
```

### Pipeline Reports
```
DAILY REPORT — [Date]

Pipeline by Stage:
[stage]: [count] | [stage]: [count] | ...
Total: [X]

Today:
- Connections sent: [X]
- Responses received: [X]
- Meetings booked: [X]

Tomorrow: [top 3 priorities]
```

### Discovery Reports
```
DISCOVERY COMPLETE — [Date]

[X] drafts posted above for review.
Checkmark = approve for publishing
X = skip
Thread reply = request edits
```

---

## Tips

1. **Review daily, not hourly.** Set aside 15-20 minutes each morning to review overnight agent work. React to everything in one pass.

2. **Use threads, not new messages.** Keep feedback attached to the work it references. This creates a revision history the agent can learn from.

3. **Don't over-explain.** Agents follow instructions literally. "Make it shorter" is better than a 3-paragraph explanation of why brevity matters.

4. **Pin important messages.** Pin approved templates, brand guidelines, or standing instructions so agents can reference them.

5. **Star your channels.** Put your agent channels in your Slack sidebar favorites so they're always visible.

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
