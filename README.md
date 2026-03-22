# inspire-skills

a collection of productivity skills for [claude code](https://docs.anthropic.com/en/docs/claude-code).

## available skills

| skill | description |
|-------|-------------|
| **agent-orchestrator** | converts any task into a production-ready multi-agent orchestration prompt |
| **payme-integration** | expert-level payme payment system integration for uzbekistan's payme business platform |
| **click-integration** | expert-level click payment integration for uzbekistan's click superapp |

## installation

```bash
# add the marketplace
/plugin marketplace add ubranch/my-marketplace

# install a skill
/plugin install agent-orchestrator@inspire-skills
/plugin install payme-integration@inspire-skills
/plugin install click-integration@inspire-skills
```

## skills

### agent-orchestrator

activates automatically when you ask to parallelize work, orchestrate agents, or split tasks into parallel work streams. generates a copy-paste ready orchestration prompt that spawns specialized agent teams for any domain.

### payme-integration

activates when you mention payme, paycom, or payment integration for the uzbek market. covers both merchant API (server-to-server) and subscribe API (card tokenization + receipts), sandbox testing, error handling, fiscalization, and production deployment.

### click-integration

activates when you mention click, click.uz, or click payment integration. covers shop API (prepare/complete), merchant API (invoices, payments, tokens, reversal), payment button, inline checkout, click pass (QR POS), fiscalization, telegram bot payments, mobile SDK, and CMS plugins.

## structure

```
my-marketplace/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   ├── agent-orchestrator/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   └── skills/
│   │       └── agent-orchestrator/
│   │           └── SKILL.md
│   ├── payme-integration/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   └── skills/
│   │       └── payme-integration/
│   │           ├── SKILL.md
│   │           └── references/
│   │               ├── merchant-api.md
│   │               ├── subscribe-api.md
│   │               └── additional-reference.md
│   └── click-integration/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           └── click-integration/
│               ├── SKILL.md
│               └── references/
│                   └── (15 reference files)
```

## license

mit
