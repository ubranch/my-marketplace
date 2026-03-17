# inspire-skills

a collection of productivity skills for [claude code](https://docs.anthropic.com/en/docs/claude-code).

## available skills

| skill | description |
|-------|-------------|
| **agent-orchestrator** | converts any task into a production-ready multi-agent orchestration prompt |

## installation

```bash
# add the marketplace
/plugin marketplace add ubranch/my-marketplace

# install a skill
/plugin install agent-orchestrator@inspire-skills
```

## usage

once installed, the `agent-orchestrator` skill activates automatically when you:

- ask to parallelize work or orchestrate agents
- say things like "orchestrate this", "create agents for this", "multi-agent"
- paste a task that would benefit from being split into parallel work streams

it generates a copy-paste ready orchestration prompt that spawns specialized agent teams for any domain — coding, research, testing, auditing, debugging, and more.

## structure

```
my-marketplace/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   └── agent-orchestrator/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           └── agent-orchestrator/
│               └── SKILL.md
```

## license

mit
