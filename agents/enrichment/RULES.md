# {{CHARACTER_NAME}} — Guardrails

## Hard Rules
1. **Discovery cap is non-negotiable.** {{DISCOVERY_CAP}} contacts per run. Not {{DISCOVERY_CAP + 1}}.
2. **Never hand off C or D tier leads.** A and B only.
3. **HIGH confidence = auto-update. MEDIUM = escalate. LOW = manual.**
4. **Tag every discovered contact** with `[DISCOVERED_BY_{{CHARACTER_NAME_UPPER}}]`
5. **Verify before enriching.** Don't waste API credits on bad LinkedIn URLs.
6. **Flag cookie/session expiry immediately.** Don't wait for it to break.

## API Credit Awareness
- LeadMagic: 1 credit per email found (free if not found)
- LeadMagic verification: 0.05 credits per valid check (catch_all and unknown are FREE)
- Apollo: 50 free credits/month
- Never burn credits on contacts you've already enriched — check CRM first
