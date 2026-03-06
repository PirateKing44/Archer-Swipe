# Sterling AI Starter Kit
## Build Your Own AI Agent Team

**Price:** Free
**Version:** 2.0 | March 2026

---

## What Is This?

This is the system behind **Sterling AI** — a multi-agent AI team that manages a $50M fundraise, runs LinkedIn outreach, produces content across 5 platforms, generates video ads, and enriches CRM data. All running on a Mac Mini.

**This kit gives you the blueprints to build your own.**

You'll get the exact prompts, workflows, quality gates, and operational files we use in production — stripped of sensitive data, ready for you to customize.

---

## What's Inside

| File | What It Gives You |
|------|-------------------|
| [character-generator-prompt.md](character-generator-prompt.md) | Pick your favorite TV show. Get a fully mapped AI agent team with identity files, workflows, and Slack setup. |
| [slack-setup-guide.md](slack-setup-guide.md) | How to set up Slack channels, emoji approval workflows, and manage your AI team. |
| [system-architecture-overview.md](system-architecture-overview.md) | How all the pieces connect: agents, Slack, dashboard, cron, memory. |

### Swipe Files (Real Production Workflows)

These aren't generic templates. They're stripped-down versions of the actual operational files running our agents.

| Swipe File | What It Covers |
|------------|---------------|
| [CRM Enrichment](swipe-files/crm-enrichment.md) | Fix broken LinkedIn URLs, discover new contacts, confidence scoring, deduplication |
| [CRM Outreach](swipe-files/crm-outreach.md) | 8-stage pipeline, 5 daily workflows, message templates, humanizer rules, quality gates |
| [Content Writing](swipe-files/content-writing.md) | 6 content pipelines, 10-step startup protocol, humanizer checklist, multi-brand voice switching |
| [Social Distribution](swipe-files/social-distribution.md) | Parasite SEO across Reddit/Quora/Medium, persona framework, UTM tagging, monitoring |
| [Video/Creative](swipe-files/video-creative.md) | AI video ad pipeline with cost gates, Airtable review hub, prompt guidelines |
| [Lead Discovery](swipe-files/lead-discovery.md) | LinkedIn mining → email enrichment → ICP scoring → outreach handoff pipeline |

---

## Quick Start

### 1. Run the Character Generator (5 minutes)

Copy the prompt from [character-generator-prompt.md](character-generator-prompt.md) and paste it into ChatGPT, Claude, or any LLM with web search.

Tell it your favorite show and your business. It will:
- Research the characters
- Map them to business roles based on personality-to-function fit
- Generate full SOUL.md identity files in each character's voice
- Output a complete Slack setup and cron schedule

### 2. Set Up Slack (10 minutes)

Follow [slack-setup-guide.md](slack-setup-guide.md) to create channels and configure the emoji approval workflow.

### 3. Pick One Swipe File (15 minutes)

Choose the workflow that saves you the most time:
- **Drowning in content?** → Start with [Content Writing](swipe-files/content-writing.md)
- **Need more leads?** → Start with [CRM Outreach](swipe-files/crm-outreach.md)
- **Want to be everywhere?** → Start with [Social Distribution](swipe-files/social-distribution.md)

Customize the swipe file with your business details, plug it into your agent's system prompt, and you're running.

---

## Who Is This For?

- **Business owners** who want an AI that actually *does* work (not just chats)
- **Founders** tired of repetitive tasks eating their day
- **Operators** who want to scale without hiring more people
- **Anyone** who wants an AI that remembers context, follows rules, and delivers results

---

## What You'll Need

1. **An LLM** — ChatGPT Pro ($20/mo), Claude Pro, or any model with tool use
2. **A clear idea** of what you want your AI to do
3. **15-30 minutes** to set up your first agent
4. **Willingness to document** how you do things (the AI can't read your mind... yet)

---

## The Philosophy

The secret isn't a better AI model. It's a **better system**.

A single ChatGPT conversation is not an agent. It's a chatbot. An agent has:
- **Identity** (SOUL.md) — who it is, how it communicates, what it believes
- **Operations** (AGENTS.md) — specific workflows, schedules, API patterns
- **Guardrails** (RULES.md) — what it never does, what it always does, correction loops
- **Memory** — context files that persist across sessions
- **Review gates** — nothing goes live without your approval

That's what this kit teaches you to build.

---

## Support & Updates

- Follow along at [sterlingai.dev](https://sterlingai.dev)
- Subscribe to the Sterling AI newsletter for new templates, workflows, and build logs
- Questions? Open an issue on this repo

---

## License

This kit is free to use for building your own AI agent systems. Attribution appreciated but not required.

---

*Built by Allen Brouwer — Sterling AI*
*"The world's most dangerous AI assistant."*
