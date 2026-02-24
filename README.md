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
| 11 | DevOps | Sonnet | Releases, deployments, rollback runbooks |

### Workflow Overview

```
Phase 0: ALL read memory + research (/last30days)        MAX 30 min
Phase 1: PM writes PRD -> DA reviews -> UX + Tech Lead   (parallel after DA)
Phase 2: DA reviews designs/arch -> Design Review gate -> Devs build
Phase 3: DA code review -> Security gate -> QA gate       (includes perf testing)
Phase 4: DevOps deploys (with rollback plan) -> ALL log learnings
```

### Quality Gates

| Gate | Type | Owner | Blocks |
|------|------|-------|--------|
| Design Review | HARD | PM | Frontend Dev cannot start without PM approval of designs |
| Security | HARD | Security Engineer | QA cannot start without security sign-off |
| QA | HARD | QA Lead | DevOps cannot deploy without QA approval |
| Devil's Advocate | SOFT (continuous) | Devil's Advocate | Blocker challenges must be addressed before proceeding |

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
git clone https://github.com/nityavp/claude-agent-team.git
cd claude-agent-team

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
DESCRIPTION: [1-2 sentence description]

MCP SERVERS: Figma, Linear, GitHub (ensure all connected via /mcp)

Spawn all 11 teammates as defined in the template.
Create tasks with proper dependencies to enforce the workflow.
Start with research phase, then PM writing the PRD.
```

## Repository Structure

```
claude-agent-team/
  team-templates/
    dev-team-config.md              # Main team configuration (747 lines)
    QUICKSTART.md                   # Quick start guide with examples
    PROJECT-CLAUDE-TEMPLATE.md      # Per-project CLAUDE.md template
    research-first-mindset.md       # Research-first methodology docs
    team-memory-system.md           # Memory system specification
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
