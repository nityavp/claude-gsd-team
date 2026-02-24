# Project Template: CLAUDE.md

Copy this file to your project directory as `CLAUDE.md` to automatically configure the development team workflow.

---

# Development Team Workflow

This project uses a structured development team with quality gates, MCP integrations, and a persistent self-learning memory system.

## MCP Server Requirements

Ensure these MCP servers are connected before starting:

- **Figma** (`/mcp`) - UX creates designs, PM creates journey diagrams, Tech Lead creates architecture diagrams
- **Linear** (`/mcp`) - All roles track tasks, milestones, bugs, and gate decisions
- **GitHub** (`/mcp`) - Devs create PRs, Security reviews code, DevOps manages releases

## Persistent Memory & Self-Learning

The team uses a persistent memory system that survives across sessions and projects. Every project makes the team smarter.

**Memory location:** `~/.claude/team-memory/`
**Full docs:** `~/.claude/team-templates/team-memory-system.md`

```
~/.claude/team-memory/
  ├── learnings.md              # Master log of all learnings (append-only)
  ├── patterns.md               # Proven patterns (confirmed 2+ times)
  ├── mistakes.md               # Mistakes to never repeat (never deleted)
  ├── decisions.md              # Architecture/design decisions + outcomes
  ├── role-knowledge/           # Per-role expertise files
  │   ├── pm.md, ux.md, tech-lead.md, frontend.md,
  │   ├── backend.md, data.md, devils-advocate.md, security.md, qa.md, devops.md
  ├── project-history/          # Per-project retrospectives
  └── code-snippets/            # Reusable code patterns
```

### Memory Rules (MANDATORY for all agents)

**Before starting ANY task:**
1. Read `~/.claude/team-memory/learnings.md`
2. Read `~/.claude/team-memory/mistakes.md`
3. Read `~/.claude/team-memory/patterns.md`
4. Read `~/.claude/team-memory/role-knowledge/{your-role}.md`
5. Apply relevant knowledge to your current task

**After completing ANY task:**
1. Append learnings to `~/.claude/team-memory/learnings.md`:
   ```
   ## [DATE] - [PROJECT] - [ROLE] - [TASK]
   **What worked:** [specific detail]
   **What failed:** [specific detail]
   **Learning:** [actionable takeaway]
   **Action:** [what to do differently next time]
   ```
2. Update `~/.claude/team-memory/role-knowledge/{your-role}.md`
3. If mistake made: log in `mistakes.md` with root cause
4. If new pattern found: add to `patterns.md`

**After project completion (CEO/Team Lead + Devil's Advocate):**
1. Write retrospective to `~/.claude/team-memory/project-history/{project-name}.md`
2. Promote confirmed patterns (same issue 2x = confirmed pattern)
3. Update all role knowledge files
4. **Devil's Advocate validates memory quality:**
   - Are learnings specific and actionable (not vague)?
   - Are mistakes logged with root cause AND fix?
   - Are patterns actually confirmed (2+ occurrences)?
   - Remove/correct inaccurate entries, flag outdated ones

## Team Structure

When creating an agent team for this project, use these roles:

1. **CEO/Management** - Strategic decisions and conflict resolution; reviews Figma designs and Linear progress
2. **PM** - Product Requirements, user stories, analytics tracking plan (STARTS FIRST); creates Linear project/issues; creates journey diagrams in FigJam; approves designs (Design Review gate)
3. **UX Designer** - **Creates** wireframes, mockups, and UI designs in Figma; creates user flow diagrams in FigJam (after PRD)
4. **Tech Lead** - **Creates** architecture and flow diagrams in FigJam; defines API contracts (after PRD); reviews GitHub PRs
5. **Frontend Dev** - UI implementation from Figma design specs + API contract + analytics tracking; creates GitHub PRs
6. **Backend Dev** - API implementation per contract; creates GitHub PRs
7. **Data Engineer** - Schemas and migrations; creates GitHub PRs
8. **Devil's Advocate** - Challenges EVERY decision; reviews PRD, architecture, designs, code quality; creates challenge issues on Linear ⚠️ CONTINUOUS
9. **Security Engineer** - Reviews code on GitHub, logs findings to Linear ⚠️ HARD GATE
10. **QA Lead** - Validates: functional + visual + performance + analytics ⚠️ HARD GATE
11. **DevOps** - Creates GitHub repo + branches (develop/staging/main), deploys to staging after Security, promotes to LIVE after QA on staging + rollback runbook

## Workflow Rules (STRICT)

### Phase 0: Load Memory + Research + Repo Setup (ALL AGENTS)
1. **Every agent reads** `~/.claude/team-memory/` before starting (learnings, mistakes, patterns, solutions-radar)
2. **DevOps creates GitHub repo** (if not exists) with branching strategy:
   ```
   gh repo create [org]/[project-name] --private --description "[description]"
   # Set up branches: develop (default), staging, main
   # Configure branch protection: main (QA+Security), staging (Security), develop (1 review + CI)
   # Create GitHub environments: staging, production
   ```
3. **PM + Tech Lead run `/last30days`** to research new tools, libraries, frameworks:
   ```
   /last30days [your tech stack] best tools and practices
   /last30days [your domain] latest libraries
   /last30days [competitor] what people are saying
   ```
4. **Every agent checks** `~/.claude/team-memory/solutions-radar.md` for known solutions
5. **Apply past learnings** - check patterns, avoid known mistakes, use role knowledge
6. **Log new discoveries** to `solutions-radar.md`
7. **Set up watchlist** for continuous tracking: `last30 watch [topic] every [interval]`
8. **This is not optional** - research before building AND repo setup are mandatory

### Phase 0.5: Research-First Decision (ALL AGENTS)
For every component, feature, or technical decision:
1. **WebSearch** for existing solutions (libraries, packages, SaaS, boilerplates, UI kits)
2. **Evaluate** at least 2-3 alternatives
3. **Decide:** Use directly (80%+ fit) → Adapt (50-80% fit) → Build from scratch (last resort)
4. **Build from scratch requires** Tech Lead approval + documented justification
5. **NEVER:** Build custom auth, custom forms, custom UI components, custom ORM, custom deployment when proven solutions exist

### Phase 1: Requirements & Design
1. **PM researches** existing products/solutions in the problem space
2. **PM writes PRD** with user stories, acceptance criteria, "Solutions Evaluated" section, AND **analytics tracking plan** (events, properties, dashboards)
3. **PM creates a NEW dedicated Linear project** (NEVER use an existing/general project):
   - Search Linear first - if project exists, confirm with CEO whether to reuse or create new
   - Create project: "[Project Name]" with description
   - Create milestones: Requirements & Design → Development → Security Review → Staging & QA → Production Release
   - Convert ALL PRD user stories to Linear issues with acceptance criteria
   - Assign priorities and dependencies
   - All gate decisions (Design Review, Security, QA) tracked as Linear issues in THIS project
4. **PM creates user journey diagrams** in FigJam for key flows
5. **Devil's Advocate reviews PRD** - challenges scope, gaps, assumptions, missing edge cases, analytics plan completeness
6. **Nothing else happens** until PRD is complete AND Devil's Advocate blocker challenges are addressed
7. Once PRD + DA review done:
   - UX Designer **searches for existing UI kits/design systems** then creates wireframes in Figma **including ALL states** (happy, error, empty, loading)
   - UX Designer **creates user flow diagrams in FigJam**
   - Tech Lead **searches for existing boilerplates/starter templates** then creates architecture in FigJam
   - Tech Lead evaluates existing libraries for each feature, then defines API contracts (in parallel with UX)
   - **Devil's Advocate reviews architecture + designs** - scalability, missing states, over-engineering
8. **PM approves designs** 🔒 DESIGN REVIEW GATE - all states designed, matches PRD, accessible
   - Frontend Dev CANNOT start until PM posts "DESIGN APPROVED" on Linear

### Phase 2: Development (USE EXISTING LIBRARIES - all work on feature branches)
All developers create **feature branches** from `develop` and submit PRs to `develop`:
```
git checkout develop
git checkout -b feature/[feature-name]
# ... work ...
# PR to develop → code review → merge
```

9. **Frontend Dev** waits for:
   - **Design Review approval from PM** (DESIGN APPROVED on Linear)
   - Figma designs from UX Designer (pulls specs via Figma MCP)
   - API contracts from Tech Lead
   - Analytics tracking plan from PM
   - **MUST use** existing component library (Shadcn, Radix, etc.), form library, data fetching library
   - **MUST implement** analytics tracking per PM's tracking plan
10. **Backend Dev** waits for:
   - API contracts from Tech Lead
   - Devil's Advocate architecture review complete
   - **MUST use** existing auth library, ORM, validation library, middleware
11. **Data Engineer** waits for:
   - Architecture from Tech Lead (references FigJam diagrams)
   - Devil's Advocate architecture review complete
   - **MUST use** existing ORM/migration tool (Prisma, Drizzle, Knex)
12. **Devil's Advocate reviews ALL code PRs** - API contract compliance, test coverage, code quality, naming

### Phase 3: Security Gate 🔒
12. **Security Engineer reviews:**
   - All backend code on GitHub (on `develop` branch)
   - All data access code on GitHub
   - Authentication/authorization
   - Secrets management
   - Logs all findings as Linear issues with severity labels
13. **HARD GATE:** After Security approves → DevOps deploys `develop` → `staging`

### Phase 4: Staging Deploy + QA Gate 🔒
14. **DevOps deploys to STAGING:**
   - Merges `develop` → `staging` via PR (requires Security approval)
   - Tags: `v{version}-rc.{N}`
   - Deploys to staging environment
15. **QA Lead validates ON STAGING:**
   - All PRD acceptance criteria (tracked in Linear)
   - UI matches Figma designs (compares via Figma MCP)
   - All user flows work end-to-end on staging
   - Integration testing on staging
   - Performance testing on staging (API <500ms p95, page load <3s)
   - Analytics validation on staging
   - Logs all bugs as Linear issues with severity and reproduction steps
16. **HARD GATE:** DevOps cannot promote to LIVE until QA posts "STAGING APPROVED" on Linear

### Phase 5: Production/LIVE Deployment
17. **DevOps promotes staging → LIVE** only after QA approves on staging:
   - Merges `staging` → `main` via PR (requires QA + Security approval)
   - Tags release: `v{version}`
   - Creates GitHub release with changelog
   - Closes Linear milestone
   - Monitors production for 30 minutes post-deploy
18. **If LIVE deploy fails** → rollback to previous tag, fix on develop, re-deploy through staging

### Phase 6: Learn & Remember (ALL AGENTS)
18. **Every agent logs learnings** to `~/.claude/team-memory/learnings.md` after completing tasks
19. **Every agent updates** their role knowledge file in `~/.claude/team-memory/role-knowledge/`
20. **Every agent updates** `~/.claude/team-memory/solutions-radar.md` with tools/libraries used and ratings
21. **CEO + Devil's Advocate** write retrospective + final challenge report to `~/.claude/team-memory/project-history/{project-name}.md`
22. **Promote patterns** - if same learning appears 2+ times, add to `patterns.md`

## MCP Usage by Role

| Role | Figma | Linear | GitHub |
|------|-------|--------|--------|
| CEO/Management | Review designs | Review project status | Monitor PR activity |
| PM | **Create** journey diagrams in FigJam | **Create** project, milestones, issues | Reference code/issues in PRD |
| UX Designer | **Create** wireframes, mockups, UI designs; **Create** flow diagrams in FigJam | Update design task status | Document component specs |
| Tech Lead | **Create** architecture, system flow, data flow diagrams in FigJam | Create technical tasks | Review PRs, enforce standards |
| Frontend Dev | Pull design specs and tokens | Update task status | Create PRs |
| Backend Dev | - | Update task status | Create PRs |
| Data Engineer | - | Update task status | Create PRs for migrations |
| Devil's Advocate | Compare designs against PRD | **Create** challenge issues with severity | **Review** ALL PRs for code quality |
| Security Engineer | Reference architecture diagrams | **Create** security issues with severity | **Review** code in PRs |
| QA Lead | **Validate** UI against designs | **Log** bugs, update gate status | Verify CI/CD checks |
| DevOps | - | Close release milestones | **Manage** releases, tags |

## Quality Gate Criteria

### Security Gate Pass Criteria:
- [ ] Zero critical or high severity vulnerabilities
- [ ] All authentication flows reviewed
- [ ] Authorization checks verified
- [ ] Secrets properly managed (no hardcoded credentials)
- [ ] SQL injection prevention verified
- [ ] XSS prevention verified
- [ ] All findings logged to Linear
- [ ] Explicit "SECURITY APPROVED" sign-off posted to Linear

### Design Review Gate Pass Criteria:
- [ ] All PRD user stories have corresponding UI designs
- [ ] All states designed: happy path, error, empty, loading
- [ ] Accessibility requirements met (contrast, keyboard nav)
- [ ] Design matches brand/design system guidelines
- [ ] Devil's Advocate design review addressed
- [ ] Explicit "DESIGN APPROVED" sign-off from PM posted to Linear

### QA Gate Pass Criteria:
- [ ] All PRD acceptance criteria passing (verified against Linear issues)
- [ ] UI matches Figma designs (validated via Figma MCP)
- [ ] Zero critical or high severity bugs
- [ ] All user flows tested end-to-end
- [ ] Integration tests passing
- [ ] **Performance benchmarks met** (API response <500ms p95, page load <3s)
- [ ] **Analytics events verified** (all events from PM's tracking plan fire correctly)
- [ ] All bugs logged to Linear
- [ ] Explicit "QA APPROVED" sign-off posted to Linear

## Task Dependencies Template

Create tasks with these dependencies (track all in Linear):

```
[ALL] Read memory + Research existing solutions + Last 30 Days check
[DevOps] Create GitHub repo (if not exists) + branches (develop/staging/main) + protection rules
  └→ [PM] Research products + Write PRD (incl. "Solutions Evaluated" + analytics tracking plan)
      └→ [Devil's Advocate] Review PRD ⚠️ (scope, gaps, assumptions, analytics plan)
          └→ [UX] Research UI kits + Create wireframes incl. ALL states (blockedBy: PRD + DA review)
              └→ [Devil's Advocate] Review designs ⚠️ (missing states? accessibility?)
                  └→ [PM] Design Review 🔒 (approve designs before dev starts)
          └→ [Tech Lead] Research boilerplates + Create architecture + API contracts (blockedBy: PRD + DA review)
              └→ [Devil's Advocate] Review architecture ⚠️ (scalability, over-engineering)
                  └→ [Frontend] Build UI + analytics (feature branches → PRs to develop)
                  └→ [Backend] Build APIs (feature branches → PRs to develop)
                  └→ [Data] Write schemas (feature branches → PRs to develop)
                      └→ [Devil's Advocate] Code quality review on ALL PRs ⚠️
                          └→ [Security] Review code on GitHub 🔒 (blockedBy: Backend, Data, DA code review)
                              └→ [DevOps] Deploy develop → STAGING (tag: v{x}-rc.{N})
                                  └→ [QA] Validate ON STAGING 🔒 (functional + visual + perf + analytics)
                                      └→ [DevOps] Promote staging → LIVE (tag: v{x}) + rollback runbook
                                          └→ [ALL] Log learnings + Update solutions-radar.md
                                              └→ [CEO + DA] Retrospective + challenge report + memory validation
```

## PRD Template

The PM should create PRDs with this structure:

```markdown
# PRD: [Feature Name]

## Overview
[What are we building and why?]

## User Stories
As a [role], I want to [action] so that [benefit]
- Acceptance Criteria:
  - [ ] Criterion 1
  - [ ] Criterion 2

## Success Metrics
- Metric 1: [target]
- Metric 2: [target]

## User Flows
1. Happy path: [step by step]
2. Error cases: [what happens when things go wrong]

## Requirements
- Must have: [list]
- Should have: [list]
- Nice to have: [list]

## Out of Scope
[What we're explicitly not building]

## Solutions Evaluated (MANDATORY)
Research existing solutions before deciding to build:

| Solution | Type | Fit | Decision | Why |
|----------|------|-----|----------|-----|
| [Solution 1] | Library/SaaS/Boilerplate | 80%+ / 50-80% / <50% | Use / Adapt / Skip | [reason] |
| [Solution 2] | ... | ... | ... | ... |

**Decision:** Build from scratch / Use [solution] / Adapt [solution]
**Justification:** [Why this approach was chosen]

## Analytics Tracking Plan (MANDATORY)
Define what to track for success metrics:

| Event Name | Trigger | Properties | Dashboard |
|-----------|---------|------------|-----------|
| [event_name] | [when it fires] | [key:value pairs] | [which dashboard] |

## Linear Project (MANDATORY - dedicated per project)
- Search Linear first: does this project already exist?
  - If YES → confirm with CEO: reuse or create new
  - If NO → create NEW project (never use general/shared project)
- Create project: "[Project Name]" with description
- Create milestones per phase:
  - Phase 1: Requirements & Design
  - Phase 2: Development
  - Phase 3: Security Review
  - Phase 4: Staging & QA
  - Phase 5: Production Release
- Create issue per user story with acceptance criteria
- Create gate decision issues: "Design Review", "Security Review", "QA Sign-off"
- Assign priorities (Critical/High/Medium/Low) and dependencies

## FigJam Diagrams
- User journey diagram for each major flow
- Feature overview diagram
```

## API Contract Template

The Tech Lead should create API contracts in OpenAPI format:

```yaml
openapi: 3.0.0
info:
  title: [Feature] API
  version: 1.0.0

paths:
  /api/v1/resource:
    get:
      summary: Get resources
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Resource'
    post:
      summary: Create resource
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateResourceRequest'
      responses:
        '201':
          description: Created

components:
  schemas:
    Resource:
      type: object
      properties:
        id: {type: string}
        name: {type: string}
```

The Tech Lead should also create architecture diagrams in FigJam showing:
- System components and their interactions
- Data flow between services
- API request/response sequences
- Database schema relationships

## Security Review Checklist

The Security Engineer must verify (review code on GitHub, log findings to Linear):

- [ ] Authentication implemented correctly
- [ ] Authorization checks on all endpoints
- [ ] Input validation on all user inputs
- [ ] SQL parameterization (no string concatenation)
- [ ] XSS prevention (output encoding)
- [ ] CSRF tokens on state-changing operations
- [ ] Secrets in environment variables (not code)
- [ ] Rate limiting on public endpoints
- [ ] Logging (without sensitive data)
- [ ] Error messages don't leak internal details

## QA Test Plan Template

The QA Lead should test (validate against Figma designs and Linear criteria):

### Functional Testing
- [ ] All user stories pass acceptance criteria (from Linear)
- [ ] Happy path works end-to-end
- [ ] Error cases handled gracefully
- [ ] Edge cases covered

### Visual Testing
- [ ] UI matches Figma designs (compare via Figma MCP)
- [ ] Design tokens used correctly (colors, spacing, typography)
- [ ] Responsive design matches Figma breakpoints

### Integration Testing
- [ ] Frontend + Backend integration works
- [ ] Database operations work correctly
- [ ] External API integrations work

### Performance Testing
- [ ] API response times under 500ms (p95)
- [ ] Page load times under 3 seconds
- [ ] No memory leaks under sustained use
- [ ] Database queries optimized (no N+1, proper indexing)

### Analytics Validation
- [ ] All events from PM's tracking plan fire correctly
- [ ] Event properties contain expected data
- [ ] No duplicate or missing events
- [ ] Analytics dashboard shows correct data

### Non-Functional Testing
- [ ] Responsive design works on mobile
- [ ] Accessibility standards met (contrast, keyboard nav, screen reader)
- [ ] Browser compatibility verified

## DevOps Setup Checklist (Phase 0)

At project start, DevOps creates the GitHub repo and environment:

- [ ] GitHub repo created (if not exists): `gh repo create [org]/[project-name] --private`
- [ ] Branches created: `develop` (default), `staging`, `main`
- [ ] Branch protection configured:
  - [ ] `main`: require PR, require QA + Security approval, no direct push, no force push
  - [ ] `staging`: require PR, require Security approval, no direct push
  - [ ] `develop`: require PR, require 1 review, CI must pass
- [ ] GitHub environments created: staging, production
- [ ] CI/CD workflows configured for each environment

## DevOps Staging Deployment Checklist

Deploy to staging after Security gate passes:

- [ ] Security Engineer has given "SECURITY APPROVED" sign-off (on Linear)
- [ ] All tests passing in CI/CD on `develop` (on GitHub)
- [ ] Merge `develop` → `staging` via PR
- [ ] Tag: `v{version}-rc.{N}`
- [ ] Deploy to staging environment
- [ ] Notify QA Lead that staging is ready for validation

## DevOps Production/LIVE Deployment Checklist

Promote to LIVE only after QA validates on staging:

- [ ] QA Lead has given "STAGING APPROVED" + "QA APPROVED" sign-off (on Linear)
- [ ] All tests passing in CI/CD on `staging` (on GitHub)
- [ ] Database migrations tested (including DOWN migration for rollback)
- [ ] **Rollback runbook created and tested:**
  - [ ] Current production tagged (`pre-release-{version}`)
  - [ ] One-liner rollback command documented
  - [ ] Down migration verified
  - [ ] Rollback triggers defined (error rate >5%, p95 >3x, critical functionality broken)
- [ ] Merge `staging` → `main` via PR (requires QA + Security approval)
- [ ] Tag release: `v{version}`
- [ ] Monitoring dashboards configured with alerting thresholds
- [ ] **Post-deploy monitoring plan** (watch error rate, response times, analytics flow for 30 min)
- [ ] GitHub release created with changelog

## Communication Norms

### Status Updates
- Each teammate posts progress updates to Linear when completing tasks
- Design Review, Security, and QA broadcast gate decisions to entire team via Linear
- Devil's Advocate blocker challenges must be visible to all
- Figma design links shared on relevant Linear issues

### Escalations
- Technical questions → Tech Lead
- Scope/requirements questions → PM
- Strategic decisions → CEO/Management
- Devil's Advocate override → CEO/Management (final say)

### Conflict Resolution Protocol
When teammates disagree on approach:
1. **5 min debate** - Both sides present their case with evidence (not opinions)
2. **Tech Lead decides** technical disputes, **PM decides** product disputes
3. **If still unresolved** → CEO/Management makes final call within 10 minutes
4. **Decision is logged** to `~/.claude/team-memory/decisions.md` with both perspectives and rationale
5. **No revisiting** - once decided, the team moves forward (revisit only in retrospective)
6. **Time-box is strict** - disputes cannot block the team for more than 15 minutes total

### Gate Failures
If Design Review fails:
1. PM documents what needs to change on Linear
2. UX Designer fixes designs
3. PM re-reviews (no shortcuts)

If Security finds issues:
1. Security documents issues as Linear issues with severity
2. Backend/Data devs fix issues
3. Security re-reviews on GitHub (no shortcuts)

If QA finds bugs:
1. QA documents bugs as Linear issues with severity and reproduction steps
2. Relevant devs fix bugs
3. QA re-tests against Figma designs and Linear criteria (no shortcuts)

If Performance fails:
1. QA documents bottlenecks on Linear
2. Backend Dev optimizes (queries, caching, indexing)
3. QA re-tests performance (no shortcuts)

## Example: Starting a New Feature

```
Create an agent team using this CLAUDE.md configuration.

FEATURE: [Feature name]
DESCRIPTION: [1-2 sentences]

Ensure MCP servers are connected: Figma, Linear, GitHub
Ensure memory directory exists: ~/.claude/team-memory/

Spawn all 11 teammates and follow the workflow:
0. ALL agents read ~/.claude/team-memory/ (learnings, mistakes, patterns, solutions-radar)
0.1. DevOps creates GitHub repo (if not exists) + branches (develop/staging/main) + protection rules
0.5. PM + Tech Lead run "Last 30 Days" research for new tools in this domain
1. PM researches existing solutions, writes PRD with "Solutions Evaluated" + analytics tracking plan, creates Linear project
1.5. Devil's Advocate reviews PRD (challenges scope, assumptions, gaps, analytics plan)
2. UX searches Figma Community for UI kits, creates designs incl. ALL states (wait for PRD + DA review)
2.5. PM approves designs (DESIGN REVIEW GATE 🔒)
3. Tech Lead evaluates boilerplates/libraries, creates architecture in FigJam (wait for PRD + DA review)
3.5. Devil's Advocate reviews architecture + designs (scalability, missing states, over-engineering)
4. Frontend Dev builds from Figma specs + API contract + analytics (feature branches → PRs to develop)
5. Backend/Data devs build from architecture (feature branches → PRs to develop)
5.5. Devil's Advocate reviews ALL code PRs for quality (API compliance, tests, naming)
6. Security reviews on GitHub, logs to Linear (HARD GATE 🔒, wait for DA code review)
7. DevOps deploys develop → STAGING (tag: v{x}-rc.{N})
8. QA validates ON STAGING: functional + visual + performance + analytics (HARD GATE 🔒)
9. DevOps promotes staging → LIVE (tag: v{x}) + rollback runbook (wait for QA staging approval)
10. ALL agents log learnings + update solutions-radar.md
11. CEO + Devil's Advocate write retrospective + challenge report + validate memory quality

Enforce task dependencies strictly.
Enforce memory read/write strictly.
Enforce research-first: no building from scratch without documented justification.
Enforce all gates: Design Review (PM) → Security → QA → Deploy.
```

---

## Notes

- This workflow is designed for quality, thoroughness, and continuous improvement
- **Every project gets its own GitHub repo** with develop/staging/main branches
- **QA validates on STAGING** before anything reaches production/LIVE
- **3 hard gates exist** to prevent issues from reaching production: Design Review, Security, QA
- Do not skip or shortcut any gate
- Each role has expertise in their domain - trust them
- UX Designer CREATES designs in Figma incl. ALL states (happy, error, empty, loading)
- Tech Lead CREATES architecture diagrams in FigJam, not just writes text
- PM CREATES journey diagrams in FigJam alongside the PRD + defines analytics tracking plan
- PM APPROVES designs before Frontend starts (Design Review gate)
- Devil's Advocate reviews EVERY major decision continuously (not a single phase)
- Devil's Advocate blocker challenges MUST be addressed before proceeding
- If overriding a Devil's Advocate blocker, log decision in `~/.claude/team-memory/decisions.md`
- QA validates performance (API <500ms p95, page load <3s) + analytics events
- DevOps creates and tests rollback runbook BEFORE every deploy
- Conflict resolution: 5 min debate → Tech Lead/PM decides → CEO if unresolved → MAX 15 min total
- All task tracking, bugs, challenge issues, and gate decisions go through Linear
- When in doubt, escalate to Tech Lead or PM

### Memory System Rules
- Reading memory before starting is NOT optional - it's mandatory for every agent
- Writing learnings after tasks is NOT optional - the team's intelligence depends on it
- Mistakes are NEVER deleted from `mistakes.md` - only marked as resolved
- Patterns get promoted: 2x = confirmed, 3x = standard practice added to role knowledge
- Retrospectives are MANDATORY after every project
- The memory system is what makes this team self-learning - skip it and the team stays static

### Research-First Rules (powered by /last30days)
- NEVER build from scratch without researching existing solutions first
- Every PRD MUST include a "Solutions Evaluated" section
- PM + Tech Lead MUST run `/last30days [topic]` before every project to find current best tools
- Use watchlist for continuous tracking: `last30 watch [topic] every [interval]`
- All discovered tools/libraries MUST be logged to `~/.claude/team-memory/solutions-radar.md`
- Building from scratch requires Tech Lead approval + documented justification
- NEVER build custom: auth, forms, component library, ORM, deployment pipeline when proven solutions exist
- `solutions-radar.md` is the team's collective knowledge of what tools exist - always check it first
- After every project, rate the tools used and update `solutions-radar.md`
