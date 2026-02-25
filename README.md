# Claude Code Agent Team Template

A production-ready, reusable template for spinning up an 11-role AI agent software development team in [Claude Code](https://claude.ai/claude-code). Features built-in quality gates, persistent self-learning memory, research-first methodology, and MCP integrations with Figma, Linear, and GitHub.

## What This Is

This template configures Claude Code to orchestrate a complete software development team of 11 specialized AI agents that collaborate autonomously with enforced workflow gates, tool integrations, and institutional memory that gets smarter with every project.

### The 11 Roles

| # | Role | Model | Responsibility |
|---|------|-------|----------------|
| 1 | CEO/Management | Sonnet | Strategic oversight, milestone reviews |
| 2 | PM | Sonnet | PRD, user stories, analytics plan, Linear project, Design Review gate owner |
| 3 | UX Designer | Sonnet | Wireframes & UI in Figma (all states), flow diagrams in FigJam |
| 4 | Tech Lead | Sonnet | Architecture diagrams in FigJam, API contracts, PR reviews |
| 5 | Frontend Dev | Sonnet | Builds UI from Figma specs + API contract, implements analytics |
| 6 | Backend Dev | Sonnet | API implementation per contract, GitHub PRs |
| 7 | Data Engineer | Sonnet | Schemas, migrations, indexing strategy |
| 8 | Devil's Advocate | Sonnet | Challenges EVERY decision, reviews all artifacts, memory quality validation |
| 9 | Security Engineer | Sonnet | Code review, OWASP compliance, HARD GATE before QA |
| 10 | QA Lead | Sonnet | Functional + visual + performance + analytics testing, HARD GATE before deploy |
| 11 | DevOps | Sonnet | Creates GitHub repo + branches, staging/LIVE deployments, rollback runbooks |

### Workflow Overview

```
Phase -1:  INITIATION — CEO + PM question user → PROJECT-BRIEF.md
Phase -0.5: DISCUSSION — Per-phase gray areas → PHASE-CONTEXT.md (before each dev phase)
Phase 0:   ALL read memory + research (/last30days)        MAX 30 min
Phase 0.1: DevOps creates GitHub repo + branches (develop/staging/main)
Phase 1:   PM writes PRD (from brief) -> DA reviews -> UX + Tech Lead (parallel after DA)
Phase 2:   DA reviews designs/arch -> Design Review gate -> Devs build [FRESH AGENTS]
Phase 3:   DA code review -> Security gate
Phase 4:   DevOps deploys to STAGING -> QA validates on staging
Phase 5:   DevOps promotes staging -> LIVE -> ALL log learnings
```

### Environment Strategy

| Branch | Environment | Purpose | Deploys After |
|--------|-------------|---------|---------------|
| `develop` | Development | Active work, feature branches merge here | Code review + CI |
| `staging` | Pre-production | QA validates here before LIVE | Security gate |
| `main` | Production/LIVE | Deployed to users | QA approval on staging |

### Quality Gates

| Gate | Type | Owner | Blocks |
|------|------|-------|--------|
| Design Review | HARD | PM | Frontend Dev cannot start without PM approval of designs |
| Security | HARD | Security Engineer | Cannot deploy to staging without security sign-off |
| QA | HARD | QA Lead | Cannot promote staging to LIVE without QA approval |
| Devil's Advocate | SOFT (continuous) | Devil's Advocate | Blocker challenges must be addressed before proceeding |

## Architecture Diagrams

Interactive FigJam diagrams — click to open and edit in Figma.

### Full Workflow Pipeline
[![Full Architecture](https://img.shields.io/badge/FigJam-Full%20Workflow%20Pipeline-blue?logo=figma)](https://www.figma.com/online-whiteboard/create-diagram/04ae5832-3c0e-41f9-8b6c-10d0e6f0f088?utm_source=other&utm_content=edit_in_figjam)

Phase -1 (Initiation) → Research → Infrastructure → PRD → Design → Discussion → Build → Quality → Ship

### Role Interactions + Gates
[![Role Interactions](https://img.shields.io/badge/FigJam-Role%20Interactions%20%2B%20Gates-red?logo=figma)](https://www.figma.com/online-whiteboard/create-diagram/10dfb685-5377-4e8d-9b6e-2cbea2fa92be?utm_source=other&utm_content=edit_in_figjam)

All 11 roles, what they produce, how they connect. 3 hard gates (red) + Devil's Advocate continuous (yellow).

### Context Management + Fresh Agents
[![Context Management](https://img.shields.io/badge/FigJam-Context%20Management%20%2B%20Fresh%20Agents-purple?logo=figma)](https://www.figma.com/online-whiteboard/create-diagram/0380ca81-5fac-466d-925f-9576c3b50415?utm_source=other&utm_content=edit_in_figjam)

Orchestrator pattern, GREEN/YELLOW/ORANGE/RED context levels, deviation rules, what gets spawned fresh per role.

### Memory System + File Outputs
[![Memory System](https://img.shields.io/badge/FigJam-Memory%20System%20%2B%20File%20Outputs-green?logo=figma)](https://www.figma.com/online-whiteboard/create-diagram/cf4cec3f-d0f3-4c38-9b71-2f2b18e4fede?utm_source=other&utm_content=edit_in_figjam)

What each phase produces, persistent memory structure, self-learning loop across projects.

## Quick Start

### Prerequisites

1. **Claude Code** installed and configured
2. **MCP Servers** connected (run `/mcp` in Claude Code):
   - Figma (designs, wireframes, FigJam diagrams)
   - Linear (issue tracking, project management)
   - GitHub (repos, PRs, code reviews)

### Installation

Copy the template files to your Claude Code configuration:

```bash
# Clone the repo
git clone https://github.com/nityavp/claude-gsd-team.git
cd claude-gsd-team

# Copy templates
cp -r team-templates/ ~/.claude/team-templates/

# Copy memory system (initializes persistent learning)
cp -r team-memory/ ~/.claude/team-memory/

# Copy research skill
cp -r skills/ ~/.claude/skills/
```

### Launch a Team

Paste this into Claude Code (replace the placeholder values):

```
Create an agent team for software development using the template at ~/.claude/team-templates/dev-team-config.md

PROJECT: [YOUR PROJECT NAME]
IDEA: [Even a single sentence — the team will ask questions to understand]

MCP SERVERS: Figma, Linear, GitHub (ensure all connected via /mcp)

Spawn all 11 teammates as defined in the template.
Create tasks with proper dependencies to enforce the workflow.
Start with PROJECT INITIATION (CEO + PM question me about the project).
```

## Repository Structure

```
claude-agent-team/
  team-templates/
    dev-team-config.md              # Main team configuration
    QUICKSTART.md                   # Quick start guide with examples
    PROJECT-CLAUDE-TEMPLATE.md      # Per-project CLAUDE.md template
    research-first-mindset.md       # Research-first methodology docs
    team-memory-system.md           # Memory system specification
    initiation/                     # NEW: Initiation & discussion workflows
      project-initiation.md         # Phase -1: Dream extraction → PROJECT-BRIEF.md
      phase-discussion.md           # Per-phase gray area locking → PHASE-CONTEXT.md
    hooks/                          # NEW: Context management
      context-monitor.md            # Context rot prevention, fresh agents, deviation rules
  team-memory/
    learnings.md                    # Cross-project learnings (append-only)
    mistakes.md                     # Mistakes with root causes
    patterns.md                     # Confirmed patterns (2+ occurrences)
    decisions.md                    # Decision log with justifications
    solutions-radar.md              # Discovered tools/libraries rated
    role-knowledge/
      pm.md                         # PM-specific knowledge
      ux.md                         # UX Designer knowledge
      tech-lead.md                  # Tech Lead knowledge
      frontend.md                   # Frontend Dev knowledge
      backend.md                    # Backend Dev knowledge
      data.md                       # Data Engineer knowledge
      devils-advocate.md            # Devil's Advocate knowledge
      security.md                   # Security Engineer knowledge
      qa.md                         # QA Lead knowledge
      devops.md                     # DevOps knowledge
    project-history/                # Retrospectives from past projects
    code-snippets/                  # Reusable code patterns
  skills/
    last30days/                     # Research skill (/last30days)
      SKILL.md                      # Skill definition
      scripts/                      # Research scripts (Reddit, X, YouTube, web)
```

## Key Features

### Project Initiation & Phase Discussion (NEW)

No more jumping straight to PRD. The team asks questions first:

- **Project Initiation (Phase -1):** CEO + PM collaboratively question the user to extract their vision. Challenges vagueness, makes abstract concrete, captures What/Why/Who/Done. Produces PROJECT-BRIEF.md that feeds the PRD.
- **Phase Discussion (Phase -0.5):** Before each dev phase, identifies gray areas (specific to that phase, not generic), presents options with recommendations, and locks decisions in PHASE-CONTEXT.md. Developers never guess.
- **Scope guard:** Discussion clarifies HOW to implement, never adds new capabilities. Scope creep is captured as "deferred ideas" for future phases.

### Context Management & Fresh Agents (NEW)

Prevents context rot — the quality degradation when LLMs fill their context window:

- **Orchestrator pattern:** Main conversation stays under 30% context, spawns fresh agents for heavy work
- **Context monitoring:** GREEN (<60%) → YELLOW (60-70%) → ORANGE (70% wrap up) → RED (80% STOP)
- **Atomic commits:** Every task = one git commit with conventional format
- **Session handoff:** STATUS.md captures progress so fresh agents resume seamlessly
- **Deviation rules:** Auto-fix bugs (3 tries), auto-add small dependencies, STOP for architecture decisions

### Persistent Self-Learning Memory

Every agent reads `~/.claude/team-memory/` before starting and writes learnings after completing tasks. The team accumulates institutional knowledge across projects:

- **learnings.md** - What worked, what failed, actionable insights
- **mistakes.md** - Mistakes with root cause and fix (never deleted, marked resolved)
- **patterns.md** - Confirmed patterns (require 2+ occurrences)
- **solutions-radar.md** - Discovered tools/libraries with ratings and experience notes
- **role-knowledge/** - Per-role expertise that deepens over time

### Research-First Methodology

No agent builds from scratch without evaluating existing solutions first:

1. Check `solutions-radar.md` for previously discovered tools
2. Run `/last30days [topic]` to research what the community uses RIGHT NOW
3. Evaluate 2-3 alternatives minimum
4. Only build custom if Tech Lead approves with documented justification

### Devil's Advocate (Continuous Quality Amplifier)

The 11th role that challenges every decision across the entire workflow:
- Reviews PRD for gaps, vague criteria, missing edge cases
- Reviews architecture for over-engineering, SPOFs, wrong tool choices
- Reviews designs for missing states, accessibility, PRD compliance
- Reviews ALL code PRs for quality, API contract compliance, test coverage
- Validates memory quality at project end (specific learnings? root causes? confirmed patterns?)

### Analytics Tracking (Built-In)

- PM defines tracking plan in PRD (events, properties, success metrics)
- Frontend implements all planned analytics events
- QA validates events fire correctly with correct properties
- No PII in analytics events

### Performance Benchmarks

- API response times: <500ms p95
- Page load: <3s
- QA validates performance as part of their gate

### Rollback Protocol

DevOps creates a tested rollback runbook before every deploy:
- Pre-deploy: tag current prod, verify down migrations, document rollback command
- Post-deploy: 30-minute monitoring window
- Rollback triggers: error rate >5%, p95 >3x baseline, critical feature broken

### Conflict Resolution

5-minute debate with evidence -> Tech Lead (technical) or PM (product) decides -> CEO if unresolved -> 15 min MAX -> log to decisions.md

## Examples

### Feature Build (Authentication)

See `team-templates/QUICKSTART.md` for a complete example of building an OAuth2 authentication system with all 11 roles, gates, and memory system active.

### Full Product Build (Task Management SaaS)

See `team-templates/QUICKSTART.md` for a full product build example with 30-40 tasks, proper dependencies, and all workflow phases.

### Mini Team (Smaller Features)

For smaller features, use a 6-role mini team:
- PM, Tech Lead, Full-Stack Dev, Devil's Advocate, Security, QA

## Monitoring

| Action | How |
|--------|-----|
| View task progress | Press `Ctrl+T` in Claude Code |
| Talk to teammates | Press `Shift+Down` to cycle through agents |
| Check gate status | Ask the gate owner directly |
| View team knowledge | Read `~/.claude/team-memory/` files |

## Configuration Files

| File | Purpose | When to Use |
|------|---------|-------------|
| `dev-team-config.md` | Full team spec with all roles, gates, workflow | Starting any project |
| `QUICKSTART.md` | Quick start guide with copy-paste examples | First time setup |
| `PROJECT-CLAUDE-TEMPLATE.md` | Per-project CLAUDE.md template | Generated per-project |
| `research-first-mindset.md` | Research methodology docs | Reference |
| `team-memory-system.md` | Memory system specification | Reference |

## License

MIT
