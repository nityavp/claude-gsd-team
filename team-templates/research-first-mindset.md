# Research-First Mindset

## Core Principle

**NEVER build from scratch. ALWAYS research first.**

Every agent on this team MUST follow this decision framework before writing a single line of code, creating a single design, or making any technical decision.

---

## The Decision Framework (MANDATORY)

```
STEP 1: WHAT IS THE BEST WAY TO DO THIS?
    ↓
STEP 2: DOES AN EXISTING SOLUTION ALREADY EXIST?
    ↓
  YES → STEP 3A: Can we use it directly? → USE IT
    ↓
  YES but not perfect → STEP 3B: Can we adapt/extend it? → FORK & ADAPT
    ↓
  NO → STEP 3C: Build from scratch (LAST RESORT)
```

### Step 1: Research the Problem Space
Before doing ANYTHING, every agent must:
- **WebSearch** for current best practices (what's the industry doing?)
- **WebSearch** for recent developments (last 30 days - new tools, libraries, frameworks)
- **Check** `~/.claude/team-memory/solutions-radar.md` for previously discovered solutions
- **Check** `~/.claude/team-memory/patterns.md` for proven approaches from past projects
- **Ask:** "Has someone already solved this problem well?"

### Step 2: Evaluate Existing Solutions
For EVERY component, feature, or technical decision, search for:
- **Open-source libraries/packages** that solve the problem
- **SaaS/API services** that handle it out of the box
- **Boilerplate/starter templates** that give a head start
- **Design systems/UI kits** (for UX/Frontend)
- **Architecture patterns** that are battle-tested
- **Code examples** from reputable sources

### Step 3A: Use Directly (BEST OUTCOME)
If an existing solution fits 80%+ of requirements:
- Use it as-is
- Document the choice in `~/.claude/team-memory/solutions-radar.md`
- Note any limitations for future reference

### Step 3B: Fork & Adapt (GOOD OUTCOME)
If an existing solution fits 50-80% of requirements:
- Use it as the foundation
- Extend or customize where needed
- Document what was changed and why
- Contribute improvements back if open-source

### Step 3C: Build from Scratch (LAST RESORT)
Only build from scratch when:
- No existing solution exists for this specific problem
- Existing solutions have critical limitations that can't be worked around
- The problem is truly unique to this project
- Document WHY no existing solution worked

---

## Research Requirements by Role

### PM - Before Writing PRD
```
RESEARCH:
1. WebSearch: "[problem domain] best practices 2024 2025 2026"
2. WebSearch: "[similar product] how it works"
3. WebSearch: "best [product type] examples"
4. Check: Are there existing products that already solve this?
5. Check: What do competitors do? What's the state of the art?
6. Read: ~/.claude/team-memory/solutions-radar.md

DOCUMENT in PRD:
- "Existing Solutions Evaluated" section
- Why we're building vs buying vs adapting
- Reference implementations to learn from
```

### UX Designer - Before Creating Designs
```
RESEARCH:
1. WebSearch: "[feature type] UI patterns 2025 2026"
2. WebSearch: "[feature type] best UX examples"
3. WebSearch: "free [feature type] Figma UI kit"
4. Check: Existing design systems (Shadcn, Radix, Material, Ant Design)
5. Check: Figma Community for free templates and components
6. Read: ~/.claude/team-memory/role-knowledge/ux.md

USE EXISTING:
- Start from a proven UI kit or design system, not blank canvas
- Adapt existing patterns to the brand, don't reinvent navigation/forms/modals
- Document which design system or kit was used as the base
```

### Tech Lead - Before Designing Architecture
```
RESEARCH:
1. WebSearch: "[tech stack] boilerplate starter template 2025 2026"
2. WebSearch: "[feature] architecture best practices"
3. WebSearch: "best [language/framework] libraries for [feature]"
4. Check: GitHub trending repos in relevant tech
5. Check: Awesome lists (awesome-react, awesome-node, etc.)
6. Check: Starter templates (create-t3-app, create-next-app, etc.)
7. Read: ~/.claude/team-memory/solutions-radar.md
8. Read: ~/.claude/team-memory/role-knowledge/tech-lead.md

DECIDE:
- Can we use an existing boilerplate/starter? (PREFERRED)
- Can we use existing libraries for 80%+ of features?
- What's the minimum custom code needed?
- Document all choices in solutions-radar.md
```

### Frontend Dev - Before Building UI
```
RESEARCH:
1. WebSearch: "[component] react component library 2025 2026"
2. WebSearch: "[feature] implementation example react"
3. Check: npm/package registries for existing components
4. Check: Component libraries (Shadcn UI, Radix, Headless UI, Chakra)
5. Check: Existing hooks libraries (TanStack, SWR, React Hook Form)
6. Read: ~/.claude/team-memory/role-knowledge/frontend.md
7. Read: ~/.claude/team-memory/code-snippets/

USE EXISTING:
- Use a component library as the base (never build buttons/modals/forms from scratch)
- Use existing form libraries (React Hook Form, Formik)
- Use existing data fetching (TanStack Query, SWR)
- Use existing state management (Zustand, Jotai) only if needed
- Document what was used in solutions-radar.md
```

### Backend Dev - Before Building APIs
```
RESEARCH:
1. WebSearch: "[feature] [framework] implementation guide"
2. WebSearch: "[feature] open source API"
3. Check: Existing middleware/plugins for the framework
4. Check: Authentication libraries (Passport, Auth.js, Lucia)
5. Check: ORM/database libraries (Prisma, Drizzle, TypeORM)
6. Check: Validation libraries (Zod, Yup, Joi)
7. Read: ~/.claude/team-memory/role-knowledge/backend.md

USE EXISTING:
- Use established ORM (never write raw SQL builders from scratch)
- Use existing auth libraries (never build auth from scratch)
- Use existing validation (never write custom validators)
- Use existing rate limiting, caching, logging middleware
```

### Data Engineer - Before Designing Schemas
```
RESEARCH:
1. WebSearch: "[domain] database schema example"
2. WebSearch: "[feature] data model best practices"
3. Check: Existing schema patterns for the domain
4. Check: Migration tools (Prisma Migrate, Flyway, Knex)
5. Read: ~/.claude/team-memory/role-knowledge/data.md

USE EXISTING:
- Reference proven schema patterns for the domain
- Use established migration tools
- Don't reinvent indexing strategies - follow documented patterns
```

### Security Engineer - Before Reviewing
```
RESEARCH:
1. WebSearch: "[framework] security best practices 2025 2026"
2. WebSearch: "OWASP top 10 latest"
3. WebSearch: "[library used] known vulnerabilities"
4. Check: CVE databases for dependencies used
5. Check: Security scanning tools (Snyk, npm audit)
6. Read: ~/.claude/team-memory/role-knowledge/security.md

USE EXISTING:
- Use established security scanning tools
- Reference OWASP checklists
- Check if security headers middleware exists for the framework
```

### QA Lead - Before Writing Tests
```
RESEARCH:
1. WebSearch: "[framework] testing best practices 2025 2026"
2. WebSearch: "[feature type] test cases examples"
3. Check: Testing frameworks and utilities (Vitest, Playwright, Testing Library)
4. Check: Existing test generators or helpers
5. Read: ~/.claude/team-memory/role-knowledge/qa.md

USE EXISTING:
- Use established testing frameworks
- Reference existing test pattern libraries
- Use existing accessibility testing tools (axe-core)
```

### DevOps - Before Setting Up Infrastructure
```
RESEARCH:
1. WebSearch: "[framework] deployment guide [platform] 2025 2026"
2. WebSearch: "[framework] CI/CD template GitHub Actions"
3. Check: Existing deployment templates (Vercel, Railway, Fly.io)
4. Check: Existing CI/CD workflow templates
5. Check: Infrastructure-as-code templates (Terraform modules, Pulumi)
6. Read: ~/.claude/team-memory/role-knowledge/devops.md

USE EXISTING:
- Use platform-provided deployment (Vercel, Railway) before custom Docker
- Use existing CI/CD templates from marketplace
- Use managed services before self-hosting
```

---

## Solutions Radar

Every discovered solution, tool, library, or pattern MUST be logged to:
`~/.claude/team-memory/solutions-radar.md`

Format:
```markdown
## [Category] - [Solution Name]
**Discovered:** [date]
**Project:** [which project found it]
**URL:** [link]
**What it does:** [1-2 sentences]
**When to use:** [use case]
**When NOT to use:** [limitations]
**Our experience:** [how it worked for us, updated after use]
**Rating:** [1-5 stars after using it]
```

Categories:
- Frontend Components
- Backend Libraries
- Authentication
- Database/ORM
- Testing
- Deployment
- Design Systems
- APIs/Services
- Boilerplates/Starters
- DevOps/CI-CD
- Security Tools
- Monitoring

---

## The "Last 30 Days" Check (Powered by /last30days Skill)

The `/last30days` skill is installed at `~/.claude/skills/last30days/`. It researches any topic across Reddit, X, YouTube, and the web from the last 30 days and returns what the community is actually using, upvoting, and recommending.

### How to Use

Before EVERY project, PM and Tech Lead MUST run:

```
/last30days [tech stack] best practices and new tools
/last30days [domain] latest libraries and frameworks
/last30days [specific tool] new features and updates
/last30days [competitor product] what people are saying
```

### Examples for a Dev Team:
```
/last30days React component libraries 2026
/last30days Node.js backend frameworks and best practices
/last30days best authentication libraries for web apps
/last30days Figma design systems and UI kits
/last30days PostgreSQL vs alternatives for SaaS
/last30days CI/CD deployment best practices
/last30days [your competitor] reviews and feedback
```

### Watchlist (Always-On Research)

Use the open variant watchlist to track topics continuously:

```
last30 watch React ecosystem changes every 2 weeks
last30 watch authentication libraries monthly
last30 watch our top 3 competitors every week
last30 watch Claude Code new features weekly
last30 watch Figma MCP updates monthly
```

Run all watches: `last30 run all my watched topics`
Get briefing: `last30 briefing` for a digest of all findings

### What to Do with Results

1. **Log discoveries** to `~/.claude/team-memory/solutions-radar.md`
2. **Update role knowledge** files with new best practices
3. **Replace outdated tools** if better alternatives emerged
4. **Share findings** with the team before starting work

### Setup Requirements
- Node.js 22+ (for X search)
- OpenAI API key in `~/.config/last30days/.env`
- X search uses browser cookies (no API key needed - just be logged into x.com)
- Optional: Brave/Parallel API keys for web search

---

## Anti-Patterns (NEVER DO THESE)

1. **Building a custom auth system** when Auth.js, Lucia, Clerk, or Supabase Auth exists
2. **Building a custom form library** when React Hook Form or Formik exists
3. **Building a custom component library** when Shadcn, Radix, or Material UI exists
4. **Building a custom ORM** when Prisma or Drizzle exists
5. **Writing custom CSS for common layouts** when Tailwind exists
6. **Building a custom deployment pipeline** when Vercel/Railway one-click deploy exists
7. **Building a custom testing framework** when Vitest/Jest exists
8. **Building a custom state management** when Zustand/Jotai or even useState is enough
9. **Building a custom date library** when date-fns or dayjs exists
10. **Building a custom validation library** when Zod or Yup exists
11. **Designing UI from blank canvas** when Figma community has free kits
12. **Writing a custom migration tool** when Prisma Migrate or Knex exists

---

## Enforcement

If any agent proposes building something from scratch, they MUST answer:
1. "What existing solutions did you evaluate?"
2. "Why can't any of them work?"
3. "What's the cost of building vs using existing?"

If they can't answer all three, they must go back to research.

The Tech Lead is the final gatekeeper - no "build from scratch" decision passes without Tech Lead approval and documented justification.
