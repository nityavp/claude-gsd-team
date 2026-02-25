# Context Monitor System

Prevents context rot — the quality degradation that happens when an LLM fills its context window during long sessions. This is the single biggest reliability improvement you can add to multi-agent workflows.

## The Problem

Claude Code has ~200K tokens of context. As agents work through long tasks, the context fills with:
- File contents from reads
- Tool call results
- Previous conversation turns
- Accumulated state

Once context usage exceeds ~70%, agents start:
- Forgetting earlier instructions
- Making inconsistent decisions
- Losing track of project state
- Producing lower quality output

## The Solution: Two Mechanisms

### 1. Context Usage Warnings (All Agents)

Every agent MUST monitor their own context usage and follow these rules:

```
CONTEXT USAGE LEVELS:

0-60%   GREEN   — Full capacity. Work normally.
60-70%  YELLOW  — Caution. Avoid large file reads. Be concise.
70-80%  ORANGE  — WARNING. Wrap up current task. Commit progress.
80%+    RED     — CRITICAL. Stop immediately. Save state. Hand off.
```

**When an agent hits ORANGE (70%):**
1. Complete the current atomic task (don't leave things half-done)
2. Commit any code changes with a descriptive message
3. Write a STATUS.md or update STATE.md with:
   - What was completed
   - What remains
   - Current blockers
   - Key decisions made
4. Signal to the orchestrator that a fresh agent is needed

**When an agent hits RED (80%):**
1. STOP immediately — do not start new tasks
2. Save all state to files (not just memory)
3. Write a handoff note:
   ```
   CONTEXT LIMIT REACHED
   Completed: [what's done]
   Next steps: [what to do next]
   Files modified: [list]
   Read these before continuing: [key files]
   ```
4. The orchestrator spawns a fresh agent to continue

### 2. Fresh Subagent Spawning (Orchestrator Pattern)

The orchestrator (main conversation) stays lean. Heavy work is delegated to spawned subagents that each get a fresh context window.

**Orchestrator rules:**
- Orchestrator should stay under 30% context usage
- Never do heavy file reading or code writing in the orchestrator
- Spawn a subagent for each substantial task
- Pass file paths to subagents, not file contents
- Collect results and route to the next step

**Subagent spawning pattern:**

```
Orchestrator (stays lean, ~10-15% context):
  │
  ├── Spawn Agent: "Plan phase 1"
  │     → Gets fresh 200K context
  │     → Reads only what it needs
  │     → Returns: PLAN.md path
  │
  ├── Spawn Agent: "Execute plan 01"
  │     → Gets fresh 200K context
  │     → Reads PLAN.md + project files
  │     → Returns: SUMMARY.md path
  │
  ├── Spawn Agent: "Execute plan 02" (parallel with plan 01 if independent)
  │     → Gets fresh 200K context
  │     → Returns: SUMMARY.md path
  │
  └── Spawn Agent: "Verify phase 1"
        → Gets fresh 200K context
        → Reads code + summaries
        → Returns: VERIFICATION.md path
```

---

## Integration With Team Roles

### Which Roles Need Fresh Contexts Most

| Role | Context Risk | Recommendation |
|---|---|---|
| Frontend Dev | HIGH — reads Figma specs + component files + writes code | Spawn fresh per feature/page |
| Backend Dev | HIGH — reads schemas + writes API code + tests | Spawn fresh per endpoint group |
| Data Engineer | MEDIUM — reads schemas, writes migrations | Can often fit in one context |
| QA Lead | HIGH — reads PRD + code + runs tests | Spawn fresh per test suite |
| Security Engineer | MEDIUM — reads code but doesn't write much | Usually fits in one context |
| UX Designer | LOW — creates designs, doesn't read much code | Rarely needs fresh context |
| PM | LOW — writes PRD, doesn't read code | Rarely needs fresh context |
| Tech Lead | MEDIUM — reads code for review, creates architecture | Fresh per major review |
| Devil's Advocate | HIGH — reviews EVERYTHING | Spawn fresh per review phase |
| DevOps | LOW-MEDIUM — runs commands, configures | Usually fits in one context |
| CEO | LOW — strategic overview | Rarely needs fresh context |

### Practical Pattern for Team Workflow

**Phase 1 (PRD):**
- PM works in main context (low context usage)
- DA spawned fresh to review PRD

**Phase 2 (Design + Architecture):**
- UX works in main context or spawned
- Tech Lead spawned fresh for architecture
- DA spawned fresh to review both

**Phase 3 (Development):**
- ALWAYS spawn fresh agents for each developer task
- Frontend Dev: one fresh agent per page/component
- Backend Dev: one fresh agent per API group
- Data Engineer: one fresh agent for schema + migrations
- DA spawned fresh for code review

**Phase 4 (Security + QA):**
- Security: spawned fresh with focus on code review
- QA: spawned fresh per test area

**What to pass to spawned agents:**

```
When spawning a development agent, provide:
1. The specific task (from PLAN.md or Linear issue)
2. File paths to read (not contents)
3. API contracts / schemas to follow
4. PHASE-CONTEXT.md decisions to respect
5. Relevant patterns from team-memory
6. Git branch to work on

Do NOT pass:
- Entire conversation history
- Unrelated file contents
- Previous agents' full outputs (just summaries)
```

---

## Deviation Rules (For Spawned Agents)

When a subagent encounters something unexpected during execution:

### Rule 1: Bug Found — Auto-Fix
If a subagent encounters a bug while executing a task:
- Fix it immediately (up to 3 attempts)
- Log the fix in the task summary
- Continue with the original task

### Rule 2: Missing Dependency — Auto-Add
If a task requires something small that wasn't planned:
- Add it if it's clearly needed and takes <5 minutes
- Log the addition in the task summary
- Continue with the original task

### Rule 3: Blocker — Auto-Resolve
If a task is blocked by a fixable issue:
- Fix the blocker (up to 3 attempts)
- Log the resolution in the task summary
- Continue with the original task

### Rule 4: Architecture Decision — STOP
If a task requires an architectural decision not covered in PHASE-CONTEXT.md:
- STOP the task
- Write what decision is needed and why
- Return to the orchestrator
- The orchestrator escalates to Tech Lead / PM

**Max auto-fix attempts per task: 3**
After 3 failed attempts at any deviation, STOP and escalate.

---

## Atomic Commits (For Spawned Agents)

Every spawned development agent MUST:

1. Make one git commit per completed task
2. Use conventional commit format:
   ```
   feat(phase-02): add user profile page
   fix(phase-02): resolve auth redirect loop
   ```
3. Never combine multiple tasks in one commit
4. Commit before context limit — don't lose work

This enables:
- `git bisect` to find where things broke
- Individual task reversion without affecting others
- Clear history of what was built and when

---

## State Files (For Session Continuity)

When a session ends (context limit, user pause, or natural end), write state to files:

### STATUS.md (Per-Phase Working State)

```markdown
# Phase [X] Status

**Last updated:** [timestamp]
**Context ended:** [reason: limit/pause/complete]

## Completed
- [x] Task 1: [description]
- [x] Task 2: [description]

## In Progress
- [ ] Task 3: [description] — [what's done, what remains]

## Not Started
- [ ] Task 4: [description]
- [ ] Task 5: [description]

## Key State
- Branch: feature/phase-02-profiles
- Last commit: abc1234 "feat(02): add profile form"
- Files modified since last commit: [list]
- Blockers: [any]

## Resume Instructions
1. Read PHASE-CONTEXT.md for decisions
2. Read this STATUS.md for progress
3. Continue from Task 3: [specific next step]
```

A fresh agent spawned with this file can pick up exactly where the previous one left off.

---

## Quick Reference

```
CONTEXT MONITOR RULES:

1. Orchestrator stays under 30% — spawn agents for heavy work
2. Agents watch their context — ORANGE (70%) = wrap up, RED (80%) = stop
3. Always save state to files before context fills
4. One commit per task — never lose work
5. Pass file paths to agents, not contents
6. Deviations: auto-fix bugs (3 tries), STOP for architecture decisions
7. Every session ends with a STATUS.md handoff
```
