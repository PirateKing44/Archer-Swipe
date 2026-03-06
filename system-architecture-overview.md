# System Architecture Overview

## How the Pieces Connect

```
                    ┌─────────────────┐
                    │   YOU (Human)   │
                    │  Final Approver  │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │   Orchestrator   │
                    │  (Task Router)   │
                    └────────┬────────┘
                             │
        ┌──────────┬─────────┼─────────┬──────────┐
        │          │         │         │          │
   ┌────▼───┐ ┌───▼────┐ ┌──▼───┐ ┌──▼────┐ ┌───▼─────┐
   │Content │ │Outreach│ │Distro│ │Creative│ │Enrichment│
   │Writer  │ │Pipeline│ │Agent │ │  Lab   │ │  Agent   │
   └────┬───┘ └───┬────┘ └──┬───┘ └──┬────┘ └───┬─────┘
        │         │         │        │           │
        └─────────┴─────────┼────────┴───────────┘
                            │
                   ┌────────▼────────┐
                   │   Slack Layer    │
                   │ (Review + Comms) │
                   └────────┬────────┘
                            │
                   ┌────────▼────────┐
                   │   Dashboard      │
                   │ (CRM + Kanban)   │
                   └─────────────────┘
```

---

## The Layers

### Layer 1: You (Human)
You're the CEO. You set direction, approve work, and make final calls. Your agents don't publish, send, or execute anything without your sign-off (unless you've explicitly granted autonomy for specific tasks).

### Layer 2: Orchestrator
The traffic cop. Routes tasks to the right agent, coordinates handoffs between agents, and escalates when something needs your attention. In practice, this is either a dedicated orchestrator agent or you doing it manually through Slack and your dashboard.

### Layer 3: Specialized Agents
Each agent handles one domain. They don't overlap. They don't step on each other's toes. A content writer writes. An outreach agent does outreach. A creative agent generates visuals. Clear boundaries, clear ownership.

### Layer 4: Slack (Communication Layer)
Every agent posts their work to their dedicated Slack channel. You review, approve, reject, or request revisions using emoji reactions. Slack is the async review layer that makes the whole system work without you needing to be "always on."

### Layer 5: Dashboard (Data Layer)
Your CRM (contacts, pipeline stages, deal tracking) and Kanban board (task management, content calendar, review queue) live here. Agents read from and write to the dashboard. It's the single source of truth for what's been done, what's in progress, and what's next.

---

## How Agents Talk to Each Other

Agents don't directly message each other. Instead, they coordinate through shared systems:

```
Content Writer → finishes draft → posts to Slack → you approve →
Distribution Agent picks up approved content → publishes to platforms

Enrichment Agent → discovers new leads → creates CRM contacts →
Outreach Agent picks up approved leads → runs connection pipeline

Creative Agent → generates video assets → posts for review →
You approve → Distribution Agent or Social Manager picks up for posting
```

**The key insight:** Every handoff goes through your approval. You're never surprised by what your agents publish or send.

---

## The Cron Schedule (Automation Layer)

Agents run on schedules, not just when you tell them to. A typical daily schedule:

```
5:00 AM  — Content Writer: session start, check content calendar
6:00 AM  — Distribution Agent: discover platform opportunities
8:00 AM  — Enrichment Agent: weekly enrichment run (Fridays)
9:00 AM  — Outreach Agent: morning pipeline review
10:00 AM — Distribution Agent: check for approvals, publish
12:00 PM — Outreach Agent: warm connection nurture
1:00 PM  — Distribution Agent: check for approvals, publish
3:00 PM  — Outreach Agent: new outreach (if approved targets exist)
5:00 PM  — All agents: daily wrap-up and reporting
```

You wake up to a Slack full of work ready for your review. You approve, reject, or revise. Agents pick up your decisions and keep moving.

---

## The Memory System

Agents wake up fresh every session. They don't remember yesterday unless you give them files to read. The memory system has three layers:

### Identity Layer (loaded every session)
- **SOUL.md** — Who the agent is. Voice, personality, standards.
- **RULES.md** — Hard guardrails. What they never do. What they always do.
- **AGENTS.md** — Operational procedures. Workflows, API patterns, templates.

### Working Memory (loaded every session)
- **CONTEXT.md** — What's in progress right now. Active tasks, pending reviews, handoff notes.
- **MEMORY.md** — Curated long-term memory. Patterns, preferences, performance data.
- **LESSONS.md** — What went wrong and what was learned. Correction loop.

### Shared Knowledge (read on demand)
- Project context, people profiles, strategy docs
- Shared across all agents — everyone reads from the same source of truth
- Agents use the DATA from shared knowledge but keep their OWN voice and approach

---

## The Tech Stack (Flexible)

The architecture is tool-agnostic. Here's what matters and what you can swap:

| Component | What It Does | Options |
|-----------|-------------|---------|
| **LLM** | Powers each agent's brain | Claude, ChatGPT, Gemini, or any model with tool use |
| **Slack** | Communication + approval layer | Slack, Discord, Teams, or any chat platform |
| **Dashboard** | CRM + task management | Custom build, Airtable, Notion, or any project tool |
| **Cron** | Scheduled agent execution | System cron, n8n, Make, Zapier |
| **Memory Files** | Agent identity + context | Markdown files in any file system |
| **Scraping/APIs** | Data gathering | Apify, PhantomBuster, custom scripts |
| **Email** | Outreach campaigns | Instantly, Lemlist, custom SMTP |

**The system is the value, not the tools.** You can build this on a $20/month LLM subscription and free tiers of everything else.

---

## Getting Started: Your First 3 Steps

1. **Run the Character Generator.** Pick your show, describe your business, get your team.
2. **Set up Slack channels.** One per agent. Pin the approval workflow guide.
3. **Pick ONE agent to build first.** Don't try to build all 5 at once. Start with the one that saves you the most time (usually Content Writing or CRM Outreach). Get it working. Then add the next one.

---

*Built by Sterling AI — [sterlingai.dev](https://sterlingai.dev)*
