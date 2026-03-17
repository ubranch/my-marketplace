---
name: agent-orchestrator
description: Use this skill whenever the user wants to parallelize work, orchestrate multiple agents, use agent teams, or says things like "do it with orchestration", "write an agent team", "make it parallel", "orchestrate this", "create agents for this", "multi-agent", or asks for a task that would benefit from being split into parallel work streams. Also triggers when user pastes a task and wants it done faster/better with multiple agents. Works for any domain — coding, research, design, testing, auditing, debugging, writing, analysis.
allowed-tools: Bash, Read, Write, Edit, Glob, Grep
---

# Agent Orchestrator Skill

Converts any task description into a production-ready Claude Code agent team orchestration prompt.

## When to Use

- User describes a task and wants it done with agent orchestration
- User says "do it with orchestration", "parallel", "agent team", "multi-agent"
- User has a complex task that would benefit from parallelization
- User wants to split work across specialized agents
- User pastes a list of things to check/fix/build and wants it done fast

## Core Principle

The orchestration prompt must be COPY-PASTE READY. The user should be able to paste it directly into Claude Code and have agent teams work immediately. No placeholders unless absolutely necessary (like file paths the user must provide).

## Prompt Generation Process

### Step 1: Analyze the Task

Read the user's request and determine:
1. **Task type**: build, fix, audit, research, test, debug, migrate, review, design, analyze
2. **Scope**: how many distinct work streams can run in parallel?
3. **Dependencies**: which parts depend on others? What must run first?
4. **File conflicts**: which agents might touch the same files? (must avoid)
5. **Deliverable**: what should the final output be?

### Step 2: Determine Agent Count and Roles

Follow these rules for agent count:

| Task Complexity | Agents | Example |
|----------------|--------|---------|
| Simple (1-2 files, 1 concern) | 2-3 | Fix a bug + write test |
| Medium (3-5 files, 2-3 concerns) | 3-4 | Audit payment integration |
| Complex (many files, many concerns) | 4-6 | Full production readiness check |
| Research/exploration (many sources) | 5-10 | Analyze 9 UI libraries |

Agent roles should be:
- **Specific** — "Click Webhook Validation Agent" not "Agent 1"
- **Non-overlapping** — each agent owns distinct files/concerns
- **Named by function** — role name tells you exactly what it does

### Step 3: Determine Execution Order

Three patterns:

**Pattern A: All Parallel**
All agents run simultaneously. Use when no dependencies exist.
```
Agents 1,2,3,4 → all parallel → Lead compiles results
```

**Pattern B: Sequential then Parallel**  
One agent must finish first, then others run in parallel.
```
Agent 1 (setup/install) → then Agents 2,3,4 parallel → Agent 5 (assembly)
```

**Pattern C: Phased**
Multiple sequential phases with parallel work within each phase.
```
Phase 1: Agents 1,2 parallel → Phase 2: Agents 3,4 parallel → Phase 3: Agent 5
```

### Step 4: Write the Orchestration Prompt

Use this EXACT template structure:

```markdown
Create an agent team to [TASK SUMMARY]. [CONTEXT/URGENCY if any].

[PREREQUISITE INSTRUCTIONS — files to read first, directories to create, etc.]

Spawn [N] teammates:

1. **[Role Name] Agent** — [One-line summary]:
   - [Specific task with exact file path and line numbers when known]
   - [Specific task]
   - [Specific task]
   - [Expected output/deliverable from this agent]

2. **[Role Name] Agent** — [One-line summary]:
   - [Specific task]
   - [Specific task]
   - [Expected output/deliverable]

[... more agents ...]

RULES:
- [Execution order: which agents run first, which parallel, which last]
- [File conflict prevention: which agent owns which files]
- [Quality gates: what to verify after completion]
- [Output format: what the final deliverable looks like]

FINAL DELIVERABLE:
[Exact format of the combined output — table, checklist, report, etc.]
```

### Step 5: Apply Quality Checks

Before outputting, verify the prompt has:

**Specificity Checklist:**
- [ ] Every agent has exact file paths (not vague "check the code")
- [ ] Every task is actionable (not "review for issues" but "check X in file Y for Z")
- [ ] Expected outputs are defined for each agent
- [ ] No two agents modify the same file (prevents merge conflicts)

**Execution Order Checklist:**
- [ ] Dependencies are explicitly stated ("Agent 1 goes FIRST")
- [ ] Parallel agents are marked ("Agents 2,3,4 run in PARALLEL")
- [ ] Final assembly agent waits for all others ("Agent 5 runs LAST")

**Completeness Checklist:**
- [ ] RULES section includes conflict prevention
- [ ] FINAL DELIVERABLE section defines exact output format
- [ ] Agent count matches task complexity (not too many, not too few)
- [ ] Each agent has 3-8 specific tasks (not 1, not 20)

## Task Type Templates

### For BUILD tasks:
```
Pattern: Installer Agent → Builder Agents (parallel) → Polish Agent
Key: Installer goes first so components are available. Builders work on different files. Polish agent assembles and tests.
```

### For FIX/DEBUG tasks:
```
Pattern: All diagnostic agents parallel → Lead compiles prioritized fix list
Key: Each agent investigates from a different angle. Cross-reference findings for confidence. Prioritize by severity.
```

### For AUDIT/CHECK tasks:
```
Pattern: All audit agents parallel → Lead produces GO/NO-GO report
Key: Each agent checks a different dimension. Use GREEN/YELLOW/RED/GRAY severity. Final deliverable is a checklist.
```

### For RESEARCH tasks:
```
Pattern: All research agents parallel → Lead synthesizes findings
Key: Each agent explores one source/site. Standard analysis template for consistency. Lead picks best patterns.
```

### For TEST tasks:
```
Pattern: Helper agent (shared fixtures) → Test agents parallel → Runner agent
Key: Shared test helpers created first. Each agent writes tests for different scope. Final agent runs all tests and reports pass/fail.
```

### For MIGRATION/REFACTOR tasks:
```
Pattern: Analysis agent → Migration agents parallel → Verification agent
Key: Analyze current state first. Each agent migrates a distinct module. Verify nothing broke after.
```

## Language Handling

- If user writes in Uzbek → generate prompt in English (Claude Code works best in English) but note user-facing text should be in Uzbek
- If user writes in Russian → same, prompt in English
- If user specifies a language for output → add to RULES section

## Anti-Patterns to Avoid

1. **Too many agents** — 10+ agents for a simple task wastes tokens and adds coordination overhead
2. **Vague tasks** — "check for issues" → be specific: "check X in file Y at line Z"
3. **File conflicts** — two agents editing the same file → explicitly assign file ownership
4. **Missing dependencies** — Agent B needs Agent A's output but runs in parallel → add ordering
5. **No deliverable** — agents work but produce no unified output → always define FINAL DELIVERABLE
6. **Giant agents** — one agent with 20 tasks → split into focused agents with 3-8 tasks each
7. **Missing context** — agents don't know what codebase/project → add "Read X first" instructions

## Examples

### Example 1: User says "audit the payment integration"
→ Detect: AUDIT task, payment domain
→ 4 agents: Protocol, Signature, Error Codes, Flow/Lifecycle
→ Pattern A: All parallel
→ Deliverable: GREEN/YELLOW/RED compliance report

### Example 2: User says "implement the billing page"
→ Detect: BUILD task, frontend domain
→ 5 agents: Installer, Pricing Cards, Calculator, Comparison Table, Polish
→ Pattern B: Installer first → 3 builders parallel → Polish last
→ Deliverable: Working billing page with all components

### Example 3: User says "will it work if I push to production?"
→ Detect: AUDIT task, production readiness
→ 4-6 agents: Env Vars, Database, Security, Client, Gateway, Infrastructure
→ Pattern A: All parallel
→ Deliverable: GO/NO-GO decision with blockers list

### Example 4: User says "review these 9 sites"
→ Detect: RESEARCH task, web browsing
→ 10 agents: 1 lead + 9 researchers (one per site)
→ Pattern A: All parallel with lead compiling
→ Deliverable: Comparison table + recommendation

## Output Format

Always output the orchestration prompt inside a single code block (```) so the user can copy-paste it directly into Claude Code. Add a brief note before the code block explaining:
- How many agents will be spawned
- Estimated time to complete
- Estimated token usage (rough: 3-5x single session for that many agents)
- Any prerequisites (files to prepare, env vars to set)
