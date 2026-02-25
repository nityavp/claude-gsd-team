# Quick Start Guide: Software Development Team

## Prerequisites

### MCP Servers

Before starting your team, ensure these MCP servers are connected:

```
/mcp   → Connect Figma (designs, wireframes, diagrams)
/mcp   → Connect Linear (issue tracking, project management)
/mcp   → Connect GitHub (code repos, PRs, reviews)
```

Teammates use these tools throughout the workflow:
- **Figma**: UX creates designs, PM creates journey diagrams, Tech Lead creates architecture diagrams
- **Linear**: All roles track tasks, log bugs, manage milestones
- **GitHub**: Devs create PRs, Security reviews code, DevOps manages releases

### Persistent Memory

The team uses a self-learning memory system at `~/.claude/team-memory/`. Every agent reads past learnings before starting and writes new learnings after completing tasks. The team gets smarter with every project.

Memory is initialized automatically. See `~/.claude/team-templates/team-memory-system.md` for full details.

### Research-First Mindset + /last30days Skill

**NEVER build from scratch.** Every agent MUST research existing solutions before building anything.

The `/last30days` skill is installed at `~/.claude/skills/last30days/`. It researches any topic across Reddit, X, YouTube, and the web to find what the community is actually using RIGHT NOW.

```
/last30days [topic]    → Research what's new and trending
last30 watch [topic]   → Add to watchlist for continuous tracking
last30 briefing        → Get digest of all watched topics
```

Decision flow: Research (`/last30days`) → Existing solution found? → YES: Use/Adapt → NO: Build (last resort)

Every discovered tool goes into `~/.claude/team-memory/solutions-radar.md`.
Full docs: `~/.claude/team-templates/research-first-mindset.md`

---

## Instant Team Setup

Copy and paste this into Claude Code to start your development team:

```
Create an agent team for software development using this structure:

PROJECT: [YOUR PROJECT NAME HERE]
IDEA: [Even a single sentence is fine — the team will ask questions to understand]

MCP SERVERS: Figma, Linear, GitHub (ensure all are connected via /mcp)

Spawn 11 teammates with these roles:

1. CEO/Management (Sonnet) - Strategic oversight, leads project initiation questioning
2. PM (Sonnet) - Assists initiation, writes PRD from PROJECT-BRIEF.md, creates user stories on Linear
3. UX Designer (Sonnet) - Creates wireframes and UI designs in Figma, creates flow diagrams in FigJam
4. Tech Lead (Sonnet) - Creates architecture diagrams in FigJam, defines API contracts, reviews GitHub PRs
5. Frontend Dev (Sonnet) - Builds UI from Figma designs + API contract [FRESH AGENT per feature]
6. Backend Dev (Sonnet) - Builds APIs using API contract [FRESH AGENT per endpoint group]
7. Data Engineer (Sonnet) - Schemas and migrations [FRESH AGENT]
8. Devil's Advocate (Sonnet) - Challenges EVERY decision, reviews PRD/architecture/designs/code ⚠️ CONTINUOUS
9. Security Engineer (Sonnet) - Reviews code on GitHub, logs issues to Linear, HARD GATE before QA
10. QA Lead (Sonnet) - Tests against PRD on Linear, validates UI against Figma, HARD GATE before deploy
11. DevOps (Sonnet) - Manages GitHub releases, deploys only after Security + QA approval

GITHUB REPO & DEPLOYMENT:
- DevOps ASKS user: "Where do you want to deploy? (Vercel, Railway, AWS, Fly.io, other)"
- DevOps creates a NEW GitHub repo for each project (if not exists)
- Branching: develop (active work) → staging (pre-prod testing) → main (LIVE/production)
- DevOps configures chosen platform: link repo, staging env, production env, env vars, domains
- All dev work happens on feature branches → PRs to develop
- QA validates on STAGING before anything reaches LIVE
- Version tags: staging = v{x}-rc.{N}, live = v{x}

PROJECT INITIATION (FIRST — before anything else):
- CEO + PM question the user collaboratively (NOT an interrogation)
- Extract: What they're building, Why, Who it's for, What "done" looks like
- Challenge vagueness: "Good UX" → "Good how?" / "Users" → "Which users?"
- Make abstract concrete: "Walk me through using this"
- Output: PROJECT-BRIEF.md at ~/.claude/project-briefs/{project-name}.md
- User confirms "yes, that captures what I want" before moving on
- For brownfield: Tech Lead scans codebase BEFORE questioning

PER-PHASE DISCUSSION (before each dev phase):
- Identify 3-5 gray areas specific to the phase (not generic categories)
- Present to user — they pick which to discuss
- Ask 3-4 focused questions per area with concrete options + recommendations
- Lock decisions in PHASE-CONTEXT.md — devs never guess
- Guard scope: discussion = HOW to implement, never adds new features
- Skip for: pure infrastructure, clear-cut from PRD, internal refactors

CONTEXT MANAGEMENT:
- Orchestrator stays under 30% context — spawns fresh agents for heavy work
- Dev agents spawned fresh per feature/page/endpoint (prevents context rot)
- Agents monitor context: GREEN (<60%) | YELLOW (60-70%) | ORANGE (70% wrap up) | RED (80% STOP)
- Every task = one atomic git commit (enables git bisect, individual revert)
- Every session ends with STATUS.md handoff (resume without losing progress)
- Deviation rules: auto-fix bugs (3 tries), STOP for architecture decisions

WORKFLOW ENFORCEMENT:
- DevOps ASKS deployment platform choice (Vercel/Railway/AWS/Fly.io/other) at project start
- DevOps creates GitHub repo + branches (develop/staging/main) + protection rules
- DevOps configures chosen deployment platform (link repo, staging env, production env)
- PROJECT INITIATION must complete FIRST — CEO + PM question user → PROJECT-BRIEF.md
- PM writes PRD from PROJECT-BRIEF.md (incl. analytics tracking plan) before UX and Tech Lead can start
- PM creates a NEW dedicated Linear project (search first - if exists, confirm with CEO whether to reuse or create new; NEVER add to general/shared project)
- PM creates milestones (Requirements → Development → Security → Staging & QA → Production) and issues from user stories
- Devil's Advocate reviews PRD BEFORE UX and Tech Lead start (challenges scope, gaps, assumptions)
- UX Designer CREATES wireframes in Figma incl. ALL states (happy, error, empty, loading)
- PM MUST approve designs before Frontend Dev starts (DESIGN REVIEW GATE 🔒)
- Tech Lead CREATES architecture diagrams in FigJam
- Devil's Advocate reviews architecture + designs BEFORE development starts
- PHASE DISCUSSION per dev phase — lock gray areas in PHASE-CONTEXT.md before devs start
- All devs spawned as FRESH AGENTS (context management) on feature branches → PRs to develop
- Each dev task = one atomic git commit (conventional format: feat/fix/refactor)
- Devil's Advocate reviews ALL code PRs for quality BEFORE Security review
- Security Engineer MUST approve before deploy to staging (HARD GATE 🔒)
- DevOps deploys develop → STAGING after Security approval
- QA Lead validates ON STAGING: functional + visual + performance + analytics (HARD GATE 🔒)
- QA Lead MUST approve staging before DevOps promotes to LIVE
- DevOps promotes staging → LIVE with rollback runbook (tested, not theoretical)
- Devil's Advocate writes final challenge report + validates memory quality

MEMORY SYSTEM:
- Every agent READS ~/.claude/team-memory/ before starting (learnings, mistakes, patterns, role knowledge)
- Every agent WRITES to ~/.claude/team-memory/ after completing tasks
- CEO + Devil's Advocate write retrospective to ~/.claude/team-memory/project-history/ when done
- Devil's Advocate validates memory quality (learnings specific? mistakes have root cause? patterns confirmed?)

RESEARCH-FIRST (MANDATORY - use /last30days skill):
- PM + Tech Lead run `/last30days [tech stack] new tools` before starting
- Check ~/.claude/team-memory/solutions-radar.md for previously discovered solutions
- Use existing libraries, UI kits, boilerplates, and services whenever possible
- Only build from scratch as LAST RESORT (needs Tech Lead approval + documented justification)
- Log all discovered solutions to ~/.claude/team-memory/solutions-radar.md
- Set up watchlist: `last30 watch [topic] every [interval]` for continuous tracking

Create tasks with proper dependencies to enforce this workflow.
Start with PROJECT INITIATION (CEO + PM questioning the user), then research, then PRD.
```

## Example: Building a New Feature

```
Create an agent team for software development using the template at ~/.claude/team-templates/dev-team-config.md

PROJECT: User Authentication System
IDEA: I want OAuth2 authentication with Google and GitHub, plus user profiles.

Spawn all 11 teammates as defined in the template.

WORKFLOW:
-1. CEO + PM: PROJECT INITIATION — question me about auth requirements, user types, session needs → PROJECT-BRIEF.md
0. ALL: Read memory + Research existing auth solutions (Auth.js, Lucia, Clerk, Supabase Auth, etc.)
0.1. DevOps: ASK "Where do you want to deploy?" + Create GitHub repo + branches + configure deployment platform
1. PM: Write PRD from PROJECT-BRIEF.md + "Solutions Evaluated" + analytics tracking plan. Create NEW Linear project.
1.5. Devil's Advocate: Review PRD - are requirements complete? Edge cases covered? Right solution chosen?
2. UX Designer: Research existing auth UI kits. Create login/signup wireframes incl. ALL states (wait for PRD + DA review)
2.5. PM: Design Review gate - approve designs before dev starts 🔒
3. Tech Lead: Evaluate auth libraries (Auth.js, Passport, Lucia). Create architecture in FigJam (wait for PRD + DA review)
3.5. Devil's Advocate: Review architecture - scalability? Over-engineering? Better alternatives?
3.7. PHASE DISCUSSION: Lock gray areas for dev (session handling? token storage? error messages?) → PHASE-CONTEXT.md
4. Frontend Dev [FRESH AGENT]: Build login UI + analytics (wait for Design Review + PHASE-CONTEXT.md)
5. Backend Dev [FRESH AGENT]: Use chosen auth library. Implement endpoints (wait for PHASE-CONTEXT.md)
6. Data Engineer [FRESH AGENT]: Use existing ORM. Design user schema (wait for PHASE-CONTEXT.md)
6.5. Devil's Advocate [FRESH AGENT]: Code quality review on ALL PRs
7. Security Engineer: Review against OWASP checklist (HARD GATE 🔒, wait for DA code review)
8. DevOps: Deploy develop → STAGING (tag: v{x}-rc.{N}) (wait for Security approval)
9. QA Lead: Validate ON STAGING - all auth flows + performance + analytics (HARD GATE 🔒)
10. DevOps: Promote staging → LIVE (tag: v{x}) + rollback runbook (wait for QA staging approval)
11. ALL: Log learnings + update solutions-radar.md
12. CEO + Devil's Advocate: Retrospective + final challenge report + memory quality validation

INITIATION: CEO + PM question me first. No PRD without PROJECT-BRIEF.md.
PHASE DISCUSSION: Lock gray areas before each dev phase. Devs never guess.
CONTEXT: Dev agents spawned fresh. Atomic commits. STATUS.md handoff.
RESEARCH-FIRST: Every agent evaluates existing solutions. No custom code without Tech Lead justification.
MEMORY: All agents read ~/.claude/team-memory/ before starting. All agents log learnings after each task.
HARD GATES: Design Review (PM), Security (before QA), QA (before deploy).
```

## Example: Full Product Build

```
Create an agent team for software development.

PROJECT: Task Management SaaS (like Asana)
DESCRIPTION: Build a collaborative task management app with teams, projects, tasks, comments, and real-time updates.

MCP SERVERS: Figma, Linear, GitHub

Spawn the full dev team (11 teammates):
- CEO/Management (Sonnet) - Reviews Figma designs and Linear progress
- PM (Sonnet) - Owns PRD, creates Linear project/issues, creates journey diagrams in FigJam
- UX Designer (Sonnet) - Creates all UI designs in Figma, flow diagrams in FigJam
- Tech Lead (Sonnet) - Creates architecture diagrams in FigJam, reviews GitHub PRs
- Frontend Dev (Sonnet) - Builds from Figma specs, creates GitHub PRs
- Backend Dev (Sonnet) - Builds APIs, creates GitHub PRs
- Data Engineer (Sonnet) - Schemas/migrations, creates GitHub PRs
- Devil's Advocate (Sonnet) - Challenges ALL decisions, reviews PRD/arch/design/code - CONTINUOUS
- Security Engineer (Sonnet) - Reviews GitHub code, logs to Linear - HARD GATE
- QA Lead (Sonnet) - Validates against Figma + Linear criteria - HARD GATE
- DevOps (Sonnet) - Manages GitHub releases

Follow the workflow from ~/.claude/team-templates/dev-team-config.md:
1. ALL agents read ~/.claude/team-memory/ first (learnings, mistakes, patterns, solutions-radar)
1.1. DevOps ASKS deployment platform + creates GitHub repo + branches + configures chosen platform
2. PM + Tech Lead run "Last 30 Days" research for new tools/solutions in the domain
3. PM writes comprehensive PRD with "Solutions Evaluated" + analytics tracking plan, sets up Linear
3.5. Devil's Advocate reviews PRD (challenges scope, assumptions, gaps, analytics plan)
4. UX uses existing UI kits + creates designs incl. ALL states. Tech Lead creates architecture in FigJam (parallel, after DA review)
4.5. Devil's Advocate reviews architecture + designs. PM approves designs (DESIGN REVIEW 🔒)
5. All devs work on feature branches → PRs to develop. USE EXISTING LIBRARIES. Frontend implements analytics.
5.5. Devil's Advocate reviews ALL code PRs for quality (API compliance, tests, naming)
6. Security reviews code on GitHub (HARD GATE 🔒, after DA code review)
7. DevOps deploys develop → STAGING (tag: v{x}-rc.{N})
8. QA validates ON STAGING: functional + visual + performance + analytics (HARD GATE 🔒)
9. DevOps promotes staging → LIVE (tag: v{x}) + rollback runbook (after QA staging approval)
10. After each task, every agent logs learnings + updates solutions-radar.md
11. CEO + Devil's Advocate write retrospective + challenge report + validate memory quality

Create 30-40 tasks with proper dependencies to build the entire product.
```

## Monitoring Your Team

### View Task Progress
Press `Ctrl+T` to see the task list with dependencies

### Talk to Teammates
Press `Shift+Down` to cycle through teammates and message them

### Check MCP Integration Status
```
Ask PM: "Show me the Linear project board with current issue statuses"
Ask UX Designer: "Share the Figma file links for completed designs"
Ask Tech Lead: "Show me the architecture diagram you created in FigJam"
```

### Check Gate Status
```
Ask Devil's Advocate: "Have you reviewed the PRD/architecture/code? Any blockers?"
Ask Security Engineer: "Have you completed your security review? Post findings to Linear."
Ask QA Lead: "Is everything passing acceptance criteria? Have you validated against Figma designs?"
```

### Enforce Gates
```
Remind the team: PM MUST approve designs before Frontend Dev starts (Design Review gate).
Remind the team: Devil's Advocate blocker challenges MUST be addressed before proceeding.
Remind the team: Security MUST approve before deploy to staging.
Remind the team: QA MUST validate ON STAGING before promoting to LIVE.
Remind DevOps: Rollback runbook MUST exist and be tested before LIVE deploy.
Remind DevOps: All dev work on feature branches → PRs to develop (never push to staging/main directly).
```

### Check Memory Usage
```
Ask any teammate: "What did you learn from past projects before starting?"
Ask any teammate: "Have you logged your learnings to ~/.claude/team-memory/?"
Ask Devil's Advocate: "Have you validated memory quality? Are learnings specific and actionable?"
```

### View Team Knowledge
```
Read ~/.claude/team-memory/learnings.md    → All learnings across projects
Read ~/.claude/team-memory/patterns.md     → Confirmed patterns
Read ~/.claude/team-memory/mistakes.md     → Mistakes to avoid
Read ~/.claude/team-memory/role-knowledge/ → Per-role expertise
```

## Troubleshooting

### QA Started Before Security Approved
```
Stop! QA Lead cannot start until Security Engineer provides explicit approval.
Security Engineer: Please provide your security review status on Linear.
```

### DevOps Trying to Deploy to LIVE Without Staging Validation
```
Stop! DevOps cannot promote to LIVE until QA validates on STAGING first.
Flow: develop → staging (after Security) → QA validates on staging → LIVE (after QA approval)
QA Lead: Have you tested on the staging environment? Post "STAGING APPROVED" on Linear.
```

### No GitHub Repo Created
```
DevOps: Every project MUST have its own GitHub repo with proper branches.
Run: gh repo create [org]/[project-name] --private
Set up branches: develop (default), staging, main
Configure branch protection rules before any code is written.
```

### Code Pushed Directly to Staging or Main
```
Stop! No direct pushes to staging or main branches.
All work must go through: feature branch → PR to develop → merge → staging → LIVE
Branch protection rules should prevent this. DevOps: verify protection rules are configured.
```

### PM Didn't Finish PRD
```
All team members: Please wait for PM to complete the PRD before starting your work.
PM: Focus on completing the PRD with user stories, acceptance criteria, and setting up the Linear project.
```

### MCP Server Not Connected
```
Run /mcp to reconnect the disconnected server (Figma, Linear, or GitHub).
Teammates cannot create designs, track issues, or manage code without MCP connections.
```

### Agent Not Reading/Writing Memory
```
Remind the agent: You MUST read ~/.claude/team-memory/ before starting work.
Remind the agent: You MUST log learnings to ~/.claude/team-memory/ after completing tasks.
This is not optional - the team's self-learning depends on every agent participating.
```

### Frontend Started Before Design Review
```
Stop! Frontend Dev cannot start until PM approves the UX designs (Design Review gate).
PM: Have you reviewed all wireframes? Are all states covered (happy, error, empty, loading)?
Post "DESIGN APPROVED" on Linear before Frontend starts.
```

### No Rollback Plan Before Deploy
```
Stop! DevOps cannot deploy without a tested rollback runbook.
DevOps: Tag current production, verify down migrations, document one-liner rollback command.
This is not optional - every deploy needs a tested escape hatch.
```

### Performance Not Tested
```
QA Lead: Performance testing is part of your gate.
Verify: API response times <500ms p95, page load <3s, analytics events firing.
Post performance results to Linear alongside your QA sign-off.
```

### Agent Building From Scratch Without Research
```
Stop! Before building anything custom, you MUST:
1. WebSearch for existing solutions (libraries, packages, services)
2. Check ~/.claude/team-memory/solutions-radar.md
3. Evaluate at least 2-3 alternatives
4. Get Tech Lead approval if no existing solution works
5. Document why existing solutions don't fit

Only build from scratch as an absolute LAST RESORT.
```

### Team Dispute Blocking Progress
```
Conflict resolution: 5 min debate with evidence → Tech Lead (technical) or PM (product) decides.
If still unresolved → CEO/Management decides within 10 min.
Log decision in ~/.claude/team-memory/decisions.md. Move on. Revisit in retrospective only.
Total dispute time: MAX 15 minutes.
```

## Advanced Usage

### Add Custom Gates
The Design Review gate (PM approves designs before Frontend starts) is already built-in. Add more gates as needed:
```
Add a CEO Design gate: CEO must also approve Figma designs alongside PM.
Add a Performance gate: Performance benchmarks must pass before deploy (separate from QA).
```

### Split into Multiple Teams
For large projects, create separate teams for different features:
```
Create Team A for user authentication (11 teammates)
Create Team B for payment processing (11 teammates)
Each team follows the same workflow independently.
Each team has its own Linear project and Figma page.
```

### Adjust Team Size
For smaller features, reduce the team:
```
Create a mini team:
- PM (Sonnet) - PRD + Linear project + FigJam journey diagrams
- Tech Lead (Sonnet) - Architecture diagrams in FigJam + API contracts
- Full-Stack Dev (Sonnet) - Builds from Figma specs, handles both frontend and backend
- Devil's Advocate (Sonnet) - Challenges PRD, architecture, and code quality - CONTINUOUS
- Security Engineer (Sonnet) - Reviews GitHub code, logs to Linear - HARD GATE
- QA Lead (Sonnet) - Validates against Figma + Linear criteria - HARD GATE
```

### Devil's Advocate Override
```
If the team overrides a Devil's Advocate blocker:
1. Decision MUST be logged in ~/.claude/team-memory/decisions.md with justification
2. CEO/Management must approve the override
3. The override becomes a "watch item" for the rest of the project
```

## Tips

1. **Connect MCP servers first:** Run `/mcp` for Figma, Linear, and GitHub before starting
2. **Research before building:** NEVER build from scratch without evaluating existing solutions
3. **Memory is mandatory:** Every agent reads before starting, writes after completing
4. **Solutions radar is gold:** `~/.claude/team-memory/solutions-radar.md` grows with every project - check it first
5. **Last 30 Days check:** PM + Tech Lead research new tools/libraries before every project
6. **Always start with PRD:** PRD must include "Solutions Evaluated" section showing what was researched
7. **UX starts from UI kits:** Use existing Figma kits/design systems as the base, not blank canvas
8. **Devs use libraries:** Component libraries, ORMs, auth libraries - never reinvent the wheel
9. **Devil's Advocate is your friend:** Every challenge makes the product better - address blocker challenges before proceeding
10. **Enforce gates strictly:** Don't let QA start without Security approval
11. **Retrospectives are mandatory:** CEO + Devil's Advocate write one after every project
12. **Update solutions-radar:** After every project, log what tools worked and what didn't

## Next Steps

1. Connect MCP servers: `/mcp` for Figma, Linear, GitHub
2. Verify memory directory exists: `~/.claude/team-memory/`
3. Read the full template: `~/.claude/team-templates/dev-team-config.md`
4. Read the research-first docs: `~/.claude/team-templates/research-first-mindset.md`
5. Read the memory system docs: `~/.claude/team-templates/team-memory-system.md`
6. Try a small project first (1-2 features)
7. After the project, review `~/.claude/team-memory/solutions-radar.md` to see what was discovered
8. Review `~/.claude/team-memory/learnings.md` to see what the team learned
