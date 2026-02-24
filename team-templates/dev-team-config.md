# Software Development Team Configuration

This is a reusable team template for software development projects with built-in quality gates, workflow enforcement, and integrated MCP (Model Context Protocol) servers for Figma, Linear, and GitHub.

## Quick Reference (TL;DR)

```
11 ROLES: CEO | PM | UX | Tech Lead | Frontend | Backend | Data | Devil's Advocate | Security | QA | DevOps

WORKFLOW IN 30 SECONDS:
  Phase 0: ALL read memory + research (/last30days) → MAX 30 min
  Phase 0.1: DevOps creates GitHub repo (if not exists) with staging/main branches
  Phase 1: PM writes PRD → DA reviews → UX designs + Tech Lead architects (parallel)
  Phase 2: DA reviews designs/arch → Design Review gate (PM approves) → Devs build (on develop branch)
  Phase 3: DA code review → Security gate 🔒 → QA gate 🔒 (includes perf testing)
  Phase 4: DevOps deploys to STAGING → QA validates on staging → Promote to LIVE
  Phase 5: ALL log learnings

ENVIRONMENTS:
  develop  → Active development (feature branches merge here)
  staging  → Pre-production testing (QA validates here before live)
  main     → Production/LIVE (only promoted from staging after QA sign-off)

3 HARD GATES:                    1 SOFT GATE:
  🔒 Design Review (PM)           ⚠️ Devil's Advocate (continuous)
  🔒 Security (blocks QA)
  🔒 QA (blocks deploy)

RESEARCH-FIRST: Never build from scratch. Check solutions-radar.md → /last30days → existing libraries.
MEMORY: Read ~/.claude/team-memory/ before starting. Write learnings after every task.
```

---

## 🔌 MCP Integrations

This template leverages three MCP servers to connect your team with essential tools:

### Installed MCP Servers
- **Figma MCP** (`https://mcp.figma.com/mcp`) - Design collaboration
- **Linear MCP** (`https://mcp.linear.app/mcp`) - Issue tracking and project management
- **GitHub MCP** (`https://api.githubcopilot.com/mcp/`) - Code repository and version control

### What This Enables
- **PM** creates and tracks Linear issues, milestones, and projects automatically; creates flow diagrams and architecture overviews in FigJam
- **UX Designer** creates wireframes, mockups, and user flow diagrams directly in Figma; pulls design tokens and specs for developer handoff
- **Tech Lead** creates architecture diagrams, system flow diagrams, and technical diagrams in FigJam; reviews GitHub PRs
- **Developers** create GitHub PRs and manage code reviews
- **Security Engineer** reviews code in GitHub repositories
- **QA Lead** logs bugs to Linear, validates implementations against Figma designs, and tracks test results
- **DevOps** manages GitHub releases and deployments

### Authentication
When teammates first use an MCP server, they'll be prompted to authenticate. Make sure you have:
- Figma account with access to relevant design files
- Linear workspace access
- GitHub account with repository permissions

---

## GitHub Repository & Environment Strategy

### Auto-Repository Creation
For every new project, DevOps **MUST** create a dedicated GitHub repository if one doesn't already exist. This happens at the very start (Phase 0), before any code is written.

```
DevOps: Create GitHub repo for this project:
  gh repo create [org]/[project-name] --private --description "[project description]"
```

### Branching Strategy (GitFlow-Lite)
```
main (LIVE/Production)
  ↑ merge via PR (only from staging, after QA + Security sign-off)
  │
staging (Pre-production)
  ↑ merge via PR (only from develop, after Security gate passes)
  │
develop (Active development)
  ↑ merge via PR (feature branches merge here)
  │
feature/[name] (Individual work)
  ↑ created from develop
```

| Branch | Purpose | Who Merges | Protection |
|--------|---------|------------|------------|
| `main` | Production/LIVE - deployed to users | DevOps only | Requires QA + Security approval, no direct push |
| `staging` | Pre-production testing environment | DevOps (after Security gate) | Requires Security approval, no direct push |
| `develop` | Integration branch for active work | Tech Lead (after code review) | Requires 1 review + CI passing |
| `feature/*` | Individual feature work | Developer (via PR to develop) | CI must pass |

### Environment Setup (DevOps creates at project start)
```
1. Create GitHub repo (if not exists)
2. Set default branch to `develop`
3. Create `staging` branch from `develop`
4. Create `main` branch from `staging`
5. Configure branch protection rules:
   - main: require PR, require QA + Security approval, no force push
   - staging: require PR, require Security approval, no force push
   - develop: require PR, require 1 review, CI must pass
6. Create GitHub environments: staging, production
7. Set up CI/CD workflows for each environment
```

### Deployment Flow
```
Feature complete → PR to develop → Code review + CI → Merge to develop
                                                           ↓
                              Security gate passes → PR to staging → Deploy to STAGING
                                                                          ↓
                                              QA validates on staging → PR to main → Deploy to LIVE
```

### Version Tagging
```
Staging deploys: v{major}.{minor}.{patch}-rc.{number}  (e.g., v1.2.0-rc.1)
Live deploys:    v{major}.{minor}.{patch}               (e.g., v1.2.0)
```

---

## Linear Project Setup (MANDATORY per Project)

**Every new project MUST get its own dedicated Linear project.** Never add issues to a general/shared project. Each project is self-contained with its own milestones, issues, and tracking.

### PM Creates Linear Project at Phase 1 (After PRD)
```
PM: Create a NEW Linear project for this project:
  1. Create project: "[Project Name]" with description
  2. Create milestones matching workflow phases:
     - Milestone: "Phase 1 - Requirements & Design"
     - Milestone: "Phase 2 - Development"
     - Milestone: "Phase 3 - Security Review"
     - Milestone: "Phase 4 - Staging & QA"
     - Milestone: "Phase 5 - Production Release"
  3. Create issues from PRD user stories (each with acceptance criteria)
  4. Create issues for technical tasks (from Tech Lead's architecture)
  5. Assign issues to milestones
  6. Set issue priorities and dependencies
```

### Issue Types (All in Project-Specific Linear Project)
| Type | Created By | Example |
|------|-----------|---------|
| User Story | PM | "As a user, I want to login with Google" |
| Technical Task | Tech Lead | "Set up database schema for users table" |
| Design Task | UX Designer | "Create login page wireframe in Figma" |
| Challenge | Devil's Advocate | "PRD missing error handling for expired tokens" |
| Security Finding | Security Engineer | "SQL injection risk in search endpoint" |
| Bug | QA Lead | "Login button unresponsive on mobile Safari" |
| Gate Decision | PM/Security/QA | "DESIGN APPROVED" / "SECURITY APPROVED" / "QA APPROVED" |
| Infrastructure | DevOps | "Set up staging environment CI/CD" |

### Linear Project Checklist
- [ ] New Linear project created (NOT in existing/general project)
- [ ] Project description matches PRD overview
- [ ] Milestones created for each workflow phase
- [ ] All PRD user stories converted to Linear issues with acceptance criteria
- [ ] Issues assigned to correct milestones
- [ ] Priorities set (Critical/High/Medium/Low)
- [ ] Dependencies between issues configured
- [ ] All gate decisions tracked as Linear issues (Design Review, Security, QA)
- [ ] All teammates know the project name and can find it in Linear

### Checking for Existing Projects
Before creating a new Linear project:
1. **Search Linear** for any existing project with the same or similar name
2. **If found:** Ask PM/CEO whether to continue in existing project or create new
3. **If new:** Always create a fresh project - never mix projects
4. **Archive old:** When a project is complete, archive it in Linear (don't delete)

---

## Team Structure (11 Roles)

### Management Layer
- **CEO/Management** - Strategic oversight and final decision authority
- **PM (Product Manager)** - Owns PRD, user stories, and acceptance criteria

### Design & Architecture Layer
- **UX Designer** - Creates wireframes and user flows (after PRD)
- **Tech Lead** - Defines architecture and API contracts (after PRD)

### Development Layer
- **Frontend Dev** - Builds UI using wireframes and API contracts
- **Backend Dev** - Builds APIs using API contracts
- **Data Engineer** - Writes schemas and migrations

### Quality & Security Layer
- **Devil's Advocate** - Challenges EVERY decision, finds flaws BEFORE they become bugs ⚠️ CONTINUOUS
- **Security Engineer** - Reviews all backend and data code ⚠️ HARD GATE before QA
- **QA Lead** - Tests against PRD acceptance criteria ⚠️ HARD GATE before deploy

### Operations Layer
- **DevOps** - Creates GitHub repo + branches, deploys to staging after Security, promotes to LIVE after QA

## Research-First Mindset (MANDATORY)

**NEVER build from scratch. ALWAYS research first.**

Full documentation: `~/.claude/team-templates/research-first-mindset.md`

Every agent MUST follow this decision framework before doing ANY work:

```
STEP 1: WHAT IS THE BEST WAY TO DO THIS?
    → WebSearch for current best practices
    → WebSearch for recent developments (last 30 days)
    → Check ~/.claude/team-memory/solutions-radar.md
    ↓
STEP 2: DOES AN EXISTING SOLUTION ALREADY EXIST?
    → Search for libraries, packages, SaaS, boilerplates, UI kits
    → Check npm, GitHub, Figma Community
    ↓
  YES (80%+ fit) → USE IT DIRECTLY
  YES (50-80% fit) → FORK & ADAPT IT
  NO existing solution → BUILD FROM SCRATCH (last resort, needs Tech Lead approval)
```

### The "Last 30 Days" Check (via /last30days Skill)
Before every project, PM and Tech Lead MUST run `/last30days` to research:
```
/last30days [your tech stack] new tools and best practices
/last30days [your domain] latest libraries and frameworks
/last30days [competitor] what people are saying
```
- Searches Reddit, X, YouTube, and web for real community discussions
- Finds what developers are actually using and recommending RIGHT NOW
- Log all findings to `~/.claude/team-memory/solutions-radar.md`
- Use watchlist for continuous tracking: `last30 watch [topic] every [interval]`

### Anti-Pattern: Building From Scratch
If any agent proposes building from scratch, they MUST answer:
1. "What existing solutions did you evaluate?"
2. "Why can't any of them work?"
3. "What's the cost of building vs using existing?"

Tech Lead is the gatekeeper - no "build from scratch" without documented justification.

---

## Workflow Dependencies

```
[ALL] Read memory + /last30days research (MAX 30 min time-box)
  [DevOps] Create GitHub repo (if not exists) + set up branches (develop/staging/main) + protection rules
    └─→ PM (PRD + "Solutions Evaluated" + analytics tracking plan)
        └─→ Devil's Advocate reviews PRD (challenges scope, gaps, assumptions)
            ├─→ UX Designer (wireframes using existing UI kits)
            │     └─→ Devil's Advocate reviews designs (missing states? matches PRD?)
            │           └─→ PM approves designs 🔒 DESIGN REVIEW GATE
            └─→ Tech Lead (architecture using existing boilerplates)
                  └─→ Devil's Advocate reviews architecture (scalability? over-engineering?)
                      ├─→ Frontend Dev (feature branches → PRs to develop)
                      ├─→ Backend Dev (feature branches → PRs to develop)
                      └─→ Data Engineer (feature branches → PRs to develop)
                              └─→ Devil's Advocate reviews code quality on ALL PRs
                                  └─→ Security Engineer (reviews security) 🔒 HARD GATE
                                      └─→ DevOps deploys develop → STAGING (tag: v{x}-rc.{N})
                                          └─→ QA Lead validates ON STAGING 🔒 HARD GATE
                                              └─→ DevOps promotes staging → LIVE (tag: v{x})
                                                  └─→ [ALL] Log learnings + update solutions-radar.md
                                                      └─→ [CEO] Retrospective + DA final report + memory validation
```

## Role Definitions

### CEO/Management
**Responsibilities:**
- Provide strategic direction and priorities
- Make final decisions on scope and trade-offs
- Approve major architectural decisions
- Resolve cross-functional conflicts

**MCP Capabilities:**
- 📋 **Linear**: Review project status, milestone progress, team velocity
- 🔧 **GitHub**: Monitor PR activity, review code quality metrics
- 📐 **Figma**: Review design progress and approve final designs

**Example MCP Commands:**
```
Show me all high-priority Linear issues for this project
List recent GitHub PRs that need my review
Show me the latest Figma designs for the homepage
```

**Outputs:** Strategic decisions, scope approvals

---

### PM (Product Manager)
**Responsibilities:**
- Write comprehensive PRD (Product Requirements Document)
- Create user stories with acceptance criteria
- Define success metrics and analytics tracking plan (what events to track, what dashboards to build)
- Validate that implementation matches requirements
- **Approve UX designs** before Frontend Dev starts (Design Review gate)
- Create user journey flow diagrams and product overview diagrams

**MCP Capabilities:**
- 📋 **Linear**: Create issues from user stories, manage project milestones, track progress, create project boards
- 🔧 **GitHub**: Reference existing code/issues in PRD, track implementation PRs
- 📐 **Figma/FigJam**: Create user journey diagrams, product flow diagrams, and feature overview diagrams in FigJam; reference design mockups in requirements

**Example MCP Commands:**
```
Create a Linear project called "[Project Name]" with these milestones
For each user story in my PRD, create a Linear issue with acceptance criteria
Create a user journey flow diagram in FigJam showing signup → onboarding → activation
Generate a feature overview diagram in FigJam
Track implementation progress by listing GitHub PRs linked to Linear issues
```

**Outputs:**
- PRD document (with analytics tracking plan)
- User stories (as Linear issues)
- Acceptance criteria
- Success metrics
- Analytics tracking plan (events, properties, dashboards)
- Linear project with milestones
- User journey diagrams (FigJam)
- Design Review sign-off (approves UX designs before dev starts)

**Blocks:** All downstream work until PRD is complete. Design Review blocks Frontend Dev.

---

### UX Designer
**Responsibilities:**
- **Create** wireframes, mockups, and full UI designs in Figma
- **Create** user flow diagrams and interaction patterns in FigJam
- Design responsive layouts and component systems
- Define design tokens (colors, spacing, typography)
- Design ALL states: happy path, error, empty, loading, offline
- Ensure accessibility standards (contrast, keyboard nav, screen reader)
- Submit designs for PM approval (Design Review gate)
- Provide developer handoff with specs and assets

**MCP Capabilities:**
- 📐 **Figma/FigJam**: **Create designs** directly in Figma using generate_figma_design; create user flow diagrams in FigJam using generate_diagram; export design specs and tokens for developer handoff; manage design system components
- 📋 **Linear**: Reference user stories while designing, update design task status, link Figma files to Linear issues
- 🔧 **GitHub**: Document component specifications for developers

**Example MCP Commands:**
```
Create a wireframe in Figma for the login page based on the PRD user stories
Generate a user flow diagram in FigJam showing the onboarding steps
Create a responsive dashboard layout in Figma with sidebar navigation
Export design tokens (colors, spacing, typography) from the design system
Update Linear issue [ISSUE-123] with link to completed Figma wireframes
Create a component library in Figma for buttons, inputs, and cards
```

**Inputs Required:** Approved PRD
**Outputs:**
- Wireframes and UI designs (**created in Figma**) - must include ALL states (happy, error, empty, loading)
- User flow diagrams (**created in FigJam**)
- Design system and component library
- Design tokens and specifications
- Developer handoff documentation

**Blocks:** Frontend Dev (via Design Review gate - PM must approve designs first)

---

### Tech Lead
**Responsibilities:**
- Design system architecture
- **Create** architecture diagrams and system flow diagrams in FigJam
- Define API contracts (OpenAPI/Swagger)
- Make technology stack decisions
- Review all technical implementations
- Ensure scalability and maintainability

**MCP Capabilities:**
- 📐 **Figma/FigJam**: **Create architecture diagrams**, system flow diagrams, data flow diagrams, and sequence diagrams in FigJam using generate_diagram; visualize microservice interactions and infrastructure topology
- 📋 **Linear**: Create technical tasks, track architecture decisions, manage tech debt issues
- 🔧 **GitHub**: Review PRs, enforce code standards, manage branch strategy

**Example MCP Commands:**
```
Create a system architecture diagram in FigJam showing microservices and their interactions
Generate a data flow diagram in FigJam for the authentication pipeline
Create an API sequence diagram in FigJam showing request/response flows
Create a database schema diagram in FigJam with table relationships
Create Linear issues for each architectural component with dependencies
```

**Inputs Required:** Approved PRD
**Outputs:**
- Architecture diagrams (**created in FigJam**)
- System flow and data flow diagrams (**created in FigJam**)
- API contracts (OpenAPI spec)
- Database schema design
- Technology stack decisions

**Blocks:** Frontend Dev, Backend Dev, Data Engineer

---

### Frontend Dev
**Responsibilities:**
- Build UI components using wireframes from Figma
- Integrate with backend APIs per contract
- Implement analytics tracking per PM's tracking plan (events, properties)
- Ensure responsive design
- Write frontend tests

**MCP Capabilities:**
- 📐 **Figma**: Pull design specs, tokens, and component details from UX Designer's Figma files using get_design_context; match implementation to designs
- 📋 **Linear**: Update task status, log blockers, track implementation progress
- 🔧 **GitHub**: Create PRs, request code reviews

**Inputs Required:**
- Wireframes from Figma (from UX Designer) - must be PM-approved (Design Review gate)
- API contracts (from Tech Lead)
- Analytics tracking plan (from PM)

**Outputs:**
- UI implementation
- Analytics tracking implementation
- Frontend test coverage
- Component documentation

**Blocks:** QA Lead (indirectly via Security)

---

### Backend Dev
**Responsibilities:**
- Build APIs per contract specification
- Implement business logic
- Write backend tests
- Ensure API performance and reliability

**MCP Capabilities:**
- 📋 **Linear**: Update task status, log blockers, track implementation progress
- 🔧 **GitHub**: Create PRs, request code reviews, reference API contract issues

**Inputs Required:** API contracts (from Tech Lead)

**Outputs:**
- API implementation
- Backend test coverage
- API documentation

**Blocks:** Security Engineer

---

### Data Engineer
**Responsibilities:**
- Design database schemas
- Write migrations
- Set up data pipelines
- Ensure data integrity and performance

**MCP Capabilities:**
- 📋 **Linear**: Update task status, log blockers, track schema/migration progress
- 🔧 **GitHub**: Create PRs for schema changes and migrations

**Inputs Required:** Architecture (from Tech Lead)

**Outputs:**
- Database schemas
- Migration scripts
- Data pipeline code
- Database documentation

**Blocks:** Security Engineer

---

### Devil's Advocate ⚠️ CONTINUOUS CHALLENGER
**Responsibilities:**
- **Challenge EVERY major decision** before it's finalized - PRD, architecture, tech choices, design
- **Review PRD for gaps** - missing edge cases, vague requirements, unrealistic scope, missing "Solutions Evaluated"
- **Challenge architecture** - single points of failure, scalability issues, over-engineering, wrong tech choices
- **Review library/tool choices** - is there a better alternative? Is this dependency maintained? License issues?
- **Challenge designs** - does this match the PRD? Is it accessible? Does it handle error states?
- **Code quality review** - bad abstractions, tech debt, copy-paste code, missing tests, poor naming
- **Verify API contracts match implementations** - Frontend and Backend must match the contract
- **Validate memory quality** - are learnings in `team-memory/` actually correct and useful?
- **Time-box enforcement** - research phase MAX 30 minutes, no analysis paralysis
- **Ask the hard questions** nobody else wants to ask

**MCP Capabilities:**
- 📋 **Linear**: Create "Challenge" issues when finding flaws, track resolution
- 🔧 **GitHub**: Review ALL PRs for code quality (not just security), flag tech debt
- 📐 **Figma**: Compare designs against PRD requirements, check for missing states

**When Devil's Advocate Reviews (CONTINUOUS - not a single phase):**
1. **After PRD** → Challenge scope, requirements, missing edge cases, solutions evaluation
2. **After Architecture** → Challenge tech choices, scalability, single points of failure
3. **After Design** → Challenge UX decisions, missing error/empty states, accessibility
4. **After Development** → Review code quality, verify API contract compliance, check test coverage
5. **After Security Review** → Second opinion on security findings
6. **After QA** → Challenge test coverage, are edge cases really tested?
7. **After Deploy** → Are monitoring/alerting/rollback procedures actually in place?

**The 5 Questions (Must ask at every review):**
1. "What happens when this fails?"
2. "What are we assuming that might be wrong?"
3. "Is there a simpler way to do this?"
4. "What did we miss?"
5. "Will this work at 10x scale?"

**Outputs:**
- Challenge reports on Linear with severity (blocker, major, minor, nit)
- Code quality review comments on GitHub PRs
- Design gap analysis
- Risk register (maintained across the project)

**Rules:**
- Devil's Advocate does NOT have veto power (unlike Security/QA gates)
- But ALL "blocker" challenges MUST be addressed before proceeding
- "Major" challenges must be acknowledged with a documented decision (accept risk or fix)
- Devil's Advocate is NOT negative for the sake of it - every challenge must include a suggested alternative
- If the team overrides a Devil's Advocate blocker, it gets logged to `~/.claude/team-memory/decisions.md` with justification

---

### Security Engineer 🔒 HARD GATE
**Responsibilities:**
- Review all backend code for security vulnerabilities
- Review data access patterns and SQL injection risks
- Check authentication and authorization
- Verify secrets management
- Must explicitly approve before QA can start

**MCP Capabilities:**
- 🔧 **GitHub**: Review code in PRs, check for security vulnerabilities, audit commits
- 📋 **Linear**: Create security issues with severity labels, track remediation, update gate status
- 📐 **Figma**: Reference architecture diagrams during review

**Inputs Required:**
- Backend implementation (from Backend Dev)
- Data code (from Data Engineer)

**Outputs:**
- Security review report
- List of vulnerabilities (must be 0 critical/high)
- Explicit sign-off (posted to Linear)

**Hard Gate:** Must approve before QA can proceed

---

### QA Lead 🔒 HARD GATE
**Responsibilities:**
- Test all functionality against PRD acceptance criteria
- Perform integration testing
- Validate frontend and backend integration
- Validate UI matches Figma designs
- **Performance testing** - measure response times, identify bottlenecks, verify under load
- **Analytics validation** - verify all events from PM's tracking plan fire correctly
- Must explicitly approve before deploy

**MCP Capabilities:**
- 📐 **Figma**: Compare implementation against original Figma designs, verify pixel accuracy and design token usage
- 📋 **Linear**: Log bugs with severity and reproduction steps, track test progress, update gate status, close validated issues
- 🔧 **GitHub**: Review test coverage in PRs, verify CI/CD checks passing

**Inputs Required:**
- Security approval (from Security Engineer)
- All implementations (Frontend, Backend, Data)
- PRD acceptance criteria (from Linear)
- Figma designs (for UI validation)
- Analytics tracking plan (from PM) - for event verification

**Outputs:**
- Test reports (functional, visual, integration)
- Performance test results (response times, load capacity, bottlenecks)
- Analytics validation report (all tracked events fire correctly)
- Bug list logged to Linear (must be 0 critical/high bugs)
- Explicit sign-off (posted to Linear)

**Hard Gate:** Must approve before DevOps can deploy

---

### DevOps
**Responsibilities:**
- **Create GitHub repo** at project start (if not exists) with branching strategy
- **Set up environments:** develop, staging, main branches with protection rules
- Set up CI/CD pipelines for staging and production
- **Deploy to staging** after Security gate passes
- **Promote to production/LIVE** only after QA validates on staging
- Monitor both staging and production health
- **Create rollback runbook** before every deploy (tested, not theoretical)
- **Set up monitoring and alerting** (error rates, response times, resource usage)
- **Manage version tags:** staging gets `-rc.N`, production gets release versions

**MCP Capabilities:**
- 🔧 **GitHub**: Create repos, manage branches, releases, tags, deployment branches, CI/CD workflows, branch protection rules
- 📋 **Linear**: Update deployment status, close release milestones, track infrastructure tasks

**Inputs Required:**
- Security sign-off (from Security Engineer) → triggers staging deploy
- QA sign-off on staging (from QA Lead) → triggers production deploy

**Outputs:**
- GitHub repository with branching strategy
- Staging deployment confirmation
- Production/LIVE deployment confirmation
- Monitoring dashboards with alerting thresholds (both environments)
- Rollback runbook (see Incident Response section below)

**Rollback Runbook (MANDATORY before every deploy):**
```
STAGING DEPLOY:
  PRE-DEPLOY:
    1. Tag: git tag v{version}-rc.{N}
    2. Verify database migration is reversible (down migration tested)
    3. Deploy to staging environment
  POST-DEPLOY:
    1. QA validates on staging (functional + visual + performance + analytics)
    2. If staging fails → fix on develop → re-deploy to staging
    3. If staging passes → QA posts "STAGING APPROVED" on Linear → proceed to LIVE

LIVE/PRODUCTION DEPLOY:
  PRE-DEPLOY:
    1. Tag current production state (git tag pre-release-{version})
    2. Merge staging → main via PR (requires QA + Security approval)
    3. Tag release: git tag v{version}
    4. Document rollback command (one-liner, no thinking required under pressure)
    5. Verify monitoring dashboards are live

  POST-DEPLOY MONITORING (first 30 minutes):
    1. Watch error rate dashboard - alert if >1% increase
    2. Watch response time dashboard - alert if p95 >2x baseline
    3. Watch resource usage - alert if CPU/memory >80%
    4. Verify analytics events are flowing

  ROLLBACK TRIGGER (any of these = immediate rollback):
    - Error rate increase >5%
    - p95 response time >3x baseline
    - Any critical functionality broken
    - Database integrity issues

  ROLLBACK STEPS:
    1. Revert to tagged release: git checkout pre-release-{version}
    2. Run down migration if needed
    3. Redeploy previous version
    4. Verify rollback success via monitoring
    5. Log incident to Linear with root cause
    6. Log to ~/.claude/team-memory/mistakes.md
```

---

## Quality Gates Enforcement

### Gate 0: Design Review (HARD GATE)
- **Blocker:** PM must review and approve UX designs before Frontend Dev starts
- **Criteria:**
  - All PRD user stories have corresponding UI designs
  - All states designed: happy path, error, empty, loading
  - Accessibility requirements met (contrast, keyboard nav)
  - Design matches brand/design system guidelines
  - Devil's Advocate design review addressed
- **Output:** Explicit "DESIGN APPROVED" sign-off (posted to Linear)
- **Blocks:** Frontend Dev cannot start until designs are approved

### Gate 1: Security Review (HARD GATE)
- **Blocker:** Security Engineer must complete review
- **Criteria:**
  - Zero critical or high severity vulnerabilities
  - All authentication/authorization verified
  - Secrets properly managed
- **Output:** Explicit "SECURITY APPROVED" sign-off
- **Blocks:** QA Lead cannot start until approved

### Gate 2: QA Validation (HARD GATE) - Validated on STAGING
- **Blocker:** QA Lead must validate all acceptance criteria **on the staging environment**
- **Criteria:**
  - All PRD acceptance criteria passing on staging
  - Zero critical or high severity bugs
  - Integration tests passing on staging
  - Performance benchmarks met on staging (API response <500ms p95, page load <3s)
  - Analytics events verified on staging (all events from tracking plan fire correctly)
  - UI matches approved Figma designs
- **Output:** Explicit "QA APPROVED" + "STAGING APPROVED" sign-off
- **Blocks:** DevOps cannot promote staging → LIVE until approved

---

## How to Instantiate This Team

### For a New Project:

1. **Start the team:**
   ```
   Create an agent team using the software development team template at ~/.claude/team-templates/dev-team-config.md

   Project: [YOUR PROJECT NAME]
   Requirements: [HIGH-LEVEL DESCRIPTION]

   Spawn all 11 teammates:
   - CEO/Management
   - PM
   - UX Designer
   - Tech Lead
   - Frontend Dev
   - Backend Dev
   - Data Engineer
   - Devil's Advocate
   - Security Engineer
   - QA Lead
   - DevOps

   Start with PM writing the PRD.
   ```

2. **Team will follow workflow automatically:**
   - PM writes PRD first
   - UX and Tech Lead wait for PRD
   - Devs wait for their inputs
   - Security gate enforced before QA
   - QA gate enforced before deploy

3. **Monitor progress:**
   - Press `Ctrl+T` to view task list
   - Use `Shift+Down` to cycle through teammates
   - Check that gates are being enforced

---

## Task Dependencies Template

When creating tasks, use this dependency structure:

```
Task 0:   ALL - Read memory + /last30days research (MAX 30 min)
Task 0.1: DevOps - Create GitHub repo (if not exists) + branches (develop/staging/main) + protection rules
Task 1:   PM - Research existing products + Write PRD with "Solutions Evaluated" + analytics tracking plan
Task 1.5: Devil's Advocate - Review PRD (gaps? scope? assumptions? analytics plan?) ⚠️
  └─> Task 2: UX - Research UI kits + Create wireframes incl. ALL states (blockedBy: Task 1.5)
      Task 2.5: Devil's Advocate - Review designs (missing states? accessibility?) ⚠️
          Task 2.7: PM - Design Review gate 🔒 (approve designs before dev starts)
  └─> Task 3: Tech Lead - Research boilerplates + Design architecture (blockedBy: Task 1.5)
      Task 3.5: Devil's Advocate - Review architecture + tech choices ⚠️
          └─> Task 4: Frontend - Build UI + analytics (feature branches → PRs to develop) (blockedBy: Task 2.7, Task 3.5)
          └─> Task 5: Backend - Build APIs (feature branches → PRs to develop) (blockedBy: Task 3.5)
          └─> Task 6: Data - Write schemas (feature branches → PRs to develop) (blockedBy: Task 3.5)
              Task 6.5: Devil's Advocate - Code quality review on ALL PRs ⚠️
                  └─> Task 7: Security - Review (blockedBy: Task 5, Task 6, Task 6.5) 🔒
                      └─> Task 8: DevOps - Deploy develop → STAGING (tag: v{x}-rc.{N}) (blockedBy: Task 7)
                          └─> Task 9: QA - Validate ON STAGING (functional + visual + perf + analytics) 🔒 (blockedBy: Task 4, Task 8)
                              └─> Task 10: DevOps - Promote staging → LIVE (tag: v{x}) + rollback runbook (blockedBy: Task 9)
                                  └─> Task 11: ALL - Log learnings + update solutions-radar.md
                                      └─> Task 12: CEO + Devil's Advocate - Retrospective + challenge report + memory validation
```

---

## Team Communication Patterns

### Status Updates
- Each teammate posts updates when completing tasks
- Security, QA, and Design Review gates must broadcast decisions to all
- Devil's Advocate blocker challenges must be visible to all

### Escalations
- Technical decisions → Tech Lead
- Scope questions → PM
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
- If Design Review finds issues → UX Designer fixes → PM re-reviews
- If Security finds issues → Backend/Data devs fix → Security re-reviews
- If QA finds bugs → Relevant devs fix → QA re-tests
- If Performance fails → Backend Dev optimizes → QA re-tests performance
- Gates must re-check after fixes (no shortcuts)

---

## Tips for Success

1. **Be explicit about gates:** Tell the team "Security MUST approve before QA starts"
2. **Size tasks appropriately:** Break work into 3-6 hour chunks
3. **Monitor gate compliance:** Check that QA doesn't start until Security approves
4. **Keep 5-6 tasks per teammate:** Prevents idle time
5. **Use split panes for visibility:** See all teammates working simultaneously

---

## Model Recommendations

- **PM, Tech Lead, Security, QA, Devil's Advocate:** Use `Sonnet` for thorough analysis
- **Frontend, Backend, Data, DevOps:** Can use `Sonnet` or `Haiku` depending on complexity
- **CEO/Management:** Use `Sonnet` for strategic decisions
- **UX Designer:** Use `Sonnet` for design thinking

---

## Persistent Memory & Self-Learning

This team uses a persistent memory system at `~/.claude/team-memory/`. Memory persists across ALL sessions and projects. The team gets smarter with every project.

Full documentation: `~/.claude/team-templates/team-memory-system.md`

### Memory Structure
```
~/.claude/team-memory/
  ├── learnings.md          # Master log (append-only, newest first)
  ├── patterns.md           # Confirmed patterns (2+ projects)
  ├── mistakes.md           # Never deleted, always growing
  ├── decisions.md          # Architecture/design decisions + outcomes
  ├── role-knowledge/       # Per-role knowledge files
  │   ├── pm.md, ux.md, tech-lead.md, frontend.md,
  │   ├── backend.md, data.md, devils-advocate.md, security.md, qa.md, devops.md
  ├── project-history/      # Per-project retrospectives
  └── code-snippets/        # Reusable code that worked
```

### MANDATORY Agent Behaviors

#### On Project Start (Every Agent):
1. **READ** `~/.claude/team-memory/learnings.md` - what did past teams learn?
2. **READ** `~/.claude/team-memory/mistakes.md` - what to avoid?
3. **READ** `~/.claude/team-memory/patterns.md` - what works?
4. **READ** `~/.claude/team-memory/role-knowledge/{your-role}.md` - role-specific knowledge
5. **APPLY** relevant learnings to your current task

#### After Completing Each Task (Every Agent):
1. **APPEND** to `~/.claude/team-memory/learnings.md`:
   ```
   ## [DATE] - [PROJECT] - [ROLE] - [TASK]
   **What worked:** [specific detail]
   **What failed:** [specific detail]
   **Learning:** [actionable takeaway]
   **Action:** [what to do differently next time]
   ```
2. **UPDATE** `~/.claude/team-memory/role-knowledge/{your-role}.md` with role-specific insight
3. **If mistake made:** Log in `~/.claude/team-memory/mistakes.md` with root cause and fix
4. **If new pattern found:** Add to `~/.claude/team-memory/patterns.md`

#### On Project Completion (CEO/Team Lead + Devil's Advocate):
1. **WRITE** retrospective to `~/.claude/team-memory/project-history/{project-name}.md`
2. **REVIEW** learnings and promote confirmed patterns
3. **UPDATE** all role knowledge files with project insights
4. **Devil's Advocate validates memory quality:**
   - Are learnings specific and actionable (not vague platitudes)?
   - Are mistakes logged with root cause AND fix (not just "it broke")?
   - Are patterns actually confirmed (2+ occurrences)?
   - Remove or correct any inaccurate entries
   - Flag outdated entries for review

### Self-Learning Loop
```
READ MEMORY → APPLY KNOWLEDGE → DO WORK → LOG LEARNINGS → UPDATE PATTERNS → REPEAT
```

### Pattern Promotion Rules
- Same issue 2x → log in `patterns.md` as CONFIRMED
- Same issue 3x → add preventive check to role's workflow
- Gate failure pattern → strengthen gate criteria automatically

### Knowledge Never Dies
- Mistakes are NEVER deleted (only marked resolved with the fix)
- Patterns grow stronger with each confirmation
- Role knowledge accumulates expertise over time
- Each project retrospective feeds the next project

---

## Customization

This template is your starting point. Customize by:
- Adding roles (e.g., Mobile Dev, ML Engineer)
- Adjusting gates (e.g., add Design Review gate)
- Changing workflow (e.g., parallel backend services)
- Modifying approval criteria
- Extending the memory system with new knowledge categories

Save your customizations as new templates in `~/.claude/team-templates/`
