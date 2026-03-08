# Sterling AI Starter Kit — Setup Guide

## What's In This Kit

```
starter-kit-giveaway/
├── SETUP.md                          ← You are here
├── .env.template                     ← All API keys in one place
├── character-generator-prompt.md     ← The fun part: map your fav universe to business roles
├── openclaw-agents-config.jsonc      ← Agent registration template for openclaw.json
│
├── agents/                           ← 6 plug-and-play agent role templates
│   ├── content-writer/               ← Blog, SEO, social copy, video scripts
│   │   ├── SOUL.md                   ← Identity & personality
│   │   ├── AGENTS.md                 ← Operating rules
│   │   ├── RULES.md                  ← Guardrails & banned words
│   │   ├── TOOLS.md                  ← API integrations & tool config
│   │   └── USER.md                   ← Who they serve
│   ├── outreach/                     ← LinkedIn, pipeline, CRM, investor/client nurture
│   ├── distribution/                 ← Parasite SEO, Reddit, Quora, Medium, cross-platform
│   ├── enrichment/                   ← CRM data quality, lead scoring, email enrichment
│   ├── creative/                     ← Video production, image generation, quality gates
│   └── social/                       ← Scheduling, trend research, engagement
│
├── skills/                           ← GTM pipeline skill swipe files (with API placeholders)
│   ├── linkedin-miner/SKILL.md       ← Mine LinkedIn engagement for warm leads
│   ├── lead-enricher/SKILL.md        ← Find verified emails (LeadMagic + Apollo)
│   ├── email-verifier/SKILL.md       ← Validate deliverability before sending
│   ├── icp-scorer/SKILL.md           ← Score & tier leads A/B/C/D
│   ├── outreach-writer/SKILL.md      ← Write personalized email sequences
│   └── instantly-loader/SKILL.md     ← Push to Instantly.ai for sending
│
└── cron/
    └── cron-templates.json           ← 9 ready-to-use scheduled job templates
```

---

## Quick Start (30 minutes to your first 3 agents)

### Step 1: Run the Character Generator (5 min)
1. Open `character-generator-prompt.md`
2. Copy the prompt into ChatGPT or Claude
3. Answer the 3 questions (your favorite universe, your business, what you need automated)
4. Save the output — you now have your character assignments

### Step 2: Set Up Your API Keys (5 min)
1. Copy `.env.template` to `.env` in your OpenClaw root
2. Fill in the keys you need:
   - **Minimum:** One LLM provider (Anthropic recommended) + Slack tokens
   - **For outreach pipeline:** Add LeadMagic + RapidAPI keys
   - **For email campaigns:** Add Instantly.ai key
   - **For creative:** Add Airtable key

### Step 3: Create Your First 3 Agents (15 min)

**Start with these three — they're the core engine:**
1. **Content Writer** — produces all your content
2. **Distribution** — puts it where your audience lives
3. **Outreach** (or Enrichment) — drives pipeline

For each agent:

**a) Create the directory:**
```bash
mkdir -p ~/.openclaw/agents/{{character-name}}/workspace/memory
mkdir -p ~/.openclaw/agents/{{character-name}}/workspace/drafts
```

**b) Copy the template files:**
```bash
cp agents/content-writer/* ~/.openclaw/agents/{{character-name}}/workspace/
```

**c) Find-and-replace the placeholders:**
Open each file and replace all `{{PLACEHOLDER}}` values:
- `{{CHARACTER_NAME}}` → Your character's name (e.g., "Lana")
- `{{BUSINESS_NAME}}` → Your company name
- `{{YOUR_NAME}}` → Your name
- `{{BRAND_1_NAME}}` → Your brand name
- `{{DASHBOARD_URL}}` → Your dashboard URL (e.g., http://localhost:3847)
- etc.

**d) Register in openclaw.json:**
Use `openclaw-agents-config.jsonc` as a reference.
Add each agent to the `agents.list` array and create Slack channel bindings.

### Step 4: Set Up Slack Channels (5 min)
1. Create a channel for each agent: `#lana`, `#cyril`, etc.
2. Get the channel IDs (right-click channel → View channel details → scroll to bottom)
3. Add the channel IDs to your openclaw.json bindings

### Step 5: Start OpenClaw
Your agents are now live. Message them in their Slack channels.

---

## Adding the GTM Pipeline (Outreach Stack)

If you selected outreach/enrichment roles, the `skills/` folder has your complete pipeline:

```
linkedin-miner → lead-enricher → email-verifier → icp-scorer → outreach-writer → instantly-loader
```

**Setup:**
1. Copy the `skills/` folder to your enrichment agent's workspace
2. Add API keys to `.env` (LeadMagic, RapidAPI, Apollo, Instantly)
3. The skills reference each other's output files automatically:
   - `02-leads.json` → `03-enriched.json` → `04-verified.json` → `05-scored.json` → `06-emails.json`
4. Each skill has approval gates — nothing sends without your sign-off

---

## Adding Cron Jobs (Automation)

1. Open `cron/cron-templates.json`
2. Replace all `{{PLACEHOLDER}}` values with your agent IDs, timezone, and model
3. Generate unique UUIDs for each job (use `uuidgen` in terminal)
4. Merge the jobs into your OpenClaw `cron/jobs.json`
5. Set `"enabled": true` on the jobs you want active
6. Start with just 2-3 cron jobs — add more as you validate they work

---

## Placeholder Reference

Every template file uses `{{PLACEHOLDER}}` syntax. Here's the complete list:

### Universal (used across all agents)
| Placeholder | What to Put | Example |
|-------------|-------------|---------|
| `{{YOUR_NAME}}` | Your name | Allen Brouwer |
| `{{YOUR_ROLE}}` | Your title | Founder & CEO |
| `{{BUSINESS_NAME}}` | Your company | Sterling AI |
| `{{DASHBOARD_URL}}` | Your dashboard URL | http://localhost:3847 |
| `{{YOUR_TIMEZONE}}` | IANA timezone | America/New_York |
| `{{YOUR_PRIMARY_MODEL}}` | Default LLM model ID | minimax-coding/MiniMax-M2.5 |

### Character (from generator output)
| Placeholder | What to Put | Example |
|-------------|-------------|---------|
| `{{CHARACTER_NAME}}` | Character name for this role | Lana |
| `{{UNIVERSE_NAME}}` | Source universe | Archer |
| `{{CHARACTER_RATIONALE}}` | Why this character fits | "Most competent, high standards, takes craft seriously" |

### Agent IDs (for openclaw.json + cron)
| Placeholder | What to Put | Example |
|-------------|-------------|---------|
| `{{CONTENT_AGENT_ID}}` | Lowercase agent ID | lana |
| `{{OUTREACH_AGENT_ID}}` | Lowercase agent ID | cyril |
| `{{DISTRIBUTION_AGENT_ID}}` | Lowercase agent ID | pam |
| `{{ENRICHMENT_AGENT_ID}}` | Lowercase agent ID | framboise |
| `{{CREATIVE_AGENT_ID}}` | Lowercase agent ID | krieger |
| `{{SOCIAL_AGENT_ID}}` | Lowercase agent ID | ray |

### Slack Channels
| Placeholder | What to Put | Example |
|-------------|-------------|---------|
| `{{SLACK_CHANNEL_ID_*}}` | Slack channel ID | C0AG66Y2RGC |
| `{{*_CHANNEL_NAME}}` | Channel name (no #) | lana |

### Outreach-Specific
| Placeholder | What to Put | Example |
|-------------|-------------|---------|
| `{{DAILY_LIMIT}}` | Daily connection cap | 20 |
| `{{HIGH_VALUE_THRESHOLD}}` | Escalation threshold | $500M+ AUM |
| `{{CONNECTION_REQUEST_TEMPLATE}}` | Your outreach template | "Hi {{first_name}}..." |
| `{{TARGET_TITLES}}` | ICP target titles | Managing Director, CIO, Partner |
| `{{TARGET_INDUSTRIES}}` | ICP industries | Wealth management, family offices |
| `{{DISCOVERY_CAP}}` | Max contacts per discovery run | 10 |

---

## FAQ

**Q: Do I need ALL the API keys?**
No. Start with an LLM provider + Slack. Add others as you activate specific skills.

**Q: Can I use different LLM providers than the ones listed?**
Yes. OpenClaw supports any OpenAI-compatible API. Update the model config in openclaw.json.

**Q: What if my favorite universe doesn't have enough characters?**
The character generator will work with what's available. You can also mix universes or use original names — the character mapping is about personality-to-function fit, not strict canon.

**Q: How many agents should I start with?**
Three. Content writer + distribution + one of (outreach or enrichment). Get those working before adding more.

**Q: Do I need a Mac Mini?**
No. OpenClaw runs on any machine — Mac, Linux, or Windows with WSL. A dedicated always-on machine is nice for cron jobs but not required.
