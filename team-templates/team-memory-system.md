# Team Memory & Self-Learning System

Every agent team MUST use this persistent memory system. Memory persists across all sessions and projects, enabling the team to learn from past work and continuously improve.

## Memory Directory Structure

```
~/.claude/team-memory/
  ├── learnings.md              # Master log of all learnings (append-only)
  ├── patterns.md               # Proven patterns and best practices
  ├── mistakes.md               # Mistakes made and how to avoid them
  ├── decisions.md              # Key architectural/design decisions and outcomes
  ├── role-knowledge/
  │   ├── pm.md                 # PM-specific learnings
  │   ├── ux.md                 # UX Designer learnings
  │   ├── tech-lead.md          # Tech Lead learnings
  │   ├── frontend.md           # Frontend Dev learnings
  │   ├── backend.md            # Backend Dev learnings
  │   ├── data.md               # Data Engineer learnings
  │   ├── security.md           # Security Engineer learnings
  │   ├── qa.md                 # QA Lead learnings
  │   └── devops.md             # DevOps learnings
  ├── project-history/
  │   └── {project-name}.md     # Per-project retrospective
  └── code-snippets/
      └── {category}.md         # Reusable code patterns that worked
```

## How It Works

### 1. READ Before Starting (Every Agent)

Before starting ANY task, every teammate MUST:

```
Read ~/.claude/team-memory/learnings.md
Read ~/.claude/team-memory/patterns.md
Read ~/.claude/team-memory/mistakes.md
Read ~/.claude/team-memory/role-knowledge/{your-role}.md
```

This ensures the team never repeats mistakes and always builds on past knowledge.

### 2. WRITE After Completing Tasks (Every Agent)

After completing any significant task, each teammate MUST append learnings:

**What to write:**
- What worked well and WHY
- What didn't work and WHY
- Any patterns discovered
- Shortcuts or optimizations found
- Gotchas or pitfalls encountered
- Better approaches discovered mid-task

**Format for appending to learnings.md:**
```markdown
## [DATE] - [PROJECT] - [ROLE] - [TASK]
**What happened:** [Brief description]
**What worked:** [What went well]
**What failed:** [What went wrong]
**Learning:** [The key takeaway - actionable and specific]
**Action:** [What to do differently next time]
```

### 3. Role-Specific Knowledge Files

Each role maintains their own knowledge file with domain-specific learnings:

**PM (pm.md):**
- PRD patterns that led to clear implementations
- User story formats that reduced ambiguity
- Estimation accuracy (estimated vs actual)
- Scope decisions that worked / caused problems

**UX Designer (ux.md):**
- Design patterns that tested well
- Figma component structures that scaled
- Handoff formats developers preferred
- Accessibility issues found repeatedly

**Tech Lead (tech-lead.md):**
- Architecture decisions and their outcomes
- API design patterns that worked
- Tech stack choices and real-world results
- Performance bottlenecks encountered

**Frontend Dev (frontend.md):**
- Component patterns that were reusable
- State management approaches and tradeoffs
- CSS/styling solutions for common layouts
- Performance optimization techniques

**Backend Dev (backend.md):**
- API patterns that scaled
- Database query optimizations
- Error handling patterns
- Authentication/authorization implementations

**Data Engineer (data.md):**
- Schema designs and migration strategies
- Indexing decisions and query performance
- Data integrity patterns
- Migration rollback procedures

**Security Engineer (security.md):**
- Common vulnerabilities found per project
- Security patterns that prevented issues
- Auth implementation review checklist (evolved)
- New attack vectors discovered

**QA Lead (qa.md):**
- Test patterns that caught the most bugs
- Edge cases that are commonly missed
- Regression areas (features that break often)
- Test automation strategies that worked

**DevOps (devops.md):**
- Deployment strategies and outcomes
- Infrastructure patterns that scaled
- Monitoring setups that caught issues early
- Rollback procedures that worked

## 4. Project Retrospective (MANDATORY)

After EVERY project completes, the CEO/Team Lead MUST create a retrospective:

**File:** `~/.claude/team-memory/project-history/{project-name}.md`

```markdown
# Retrospective: {Project Name}
**Date:** {date}
**Duration:** {time taken}
**Team Size:** {number of agents}

## What We Built
{Brief description}

## What Went Well
- {item 1}
- {item 2}

## What Went Wrong
- {item 1 with root cause}
- {item 2 with root cause}

## Key Metrics
- PRD quality: {rating 1-5}
- Design accuracy: {how close was implementation to design}
- Security issues found: {count and severity}
- QA bugs found: {count and severity}
- Deployment issues: {count}

## Learnings to Carry Forward
1. {Learning 1 - specific and actionable}
2. {Learning 2}
3. {Learning 3}

## Process Improvements
- {What to change in the workflow}
- {What gate criteria to add/remove}

## Reusable Assets
- {Code patterns to save}
- {Design components to reuse}
- {API contracts to template}
```

## 5. Self-Learning Rules

### Pattern Detection
When the SAME type of issue appears 2+ times across projects:
1. Document it in `patterns.md` as a CONFIRMED pattern
2. Add a preventive check to the relevant role's workflow
3. Update gate criteria if it's a security or quality issue

### Mistake Prevention
When a mistake is made:
1. Immediately log it in `mistakes.md`
2. Add a "BEFORE YOU START" check to the relevant role's knowledge file
3. If it's a gate failure, strengthen the gate criteria

### Knowledge Promotion
When a learning proves valuable across 3+ projects:
1. Promote it from `learnings.md` to `patterns.md`
2. Add it to the role's knowledge file as a STANDARD PRACTICE
3. Consider adding it to the team template itself

### Continuous Improvement Loop
```
START PROJECT
    → Read all memory files
    → Apply known patterns
    → Avoid known mistakes
    → Execute with awareness
    → Log new learnings
    → Update patterns
    → Write retrospective
END PROJECT → feeds into next project
```

## 6. Memory Commands for Agents

Every agent should use these when interacting with memory:

**Before starting work:**
```
Read the team memory at ~/.claude/team-memory/learnings.md
Read my role knowledge at ~/.claude/team-memory/role-knowledge/{role}.md
Read known mistakes at ~/.claude/team-memory/mistakes.md
Apply any relevant patterns from ~/.claude/team-memory/patterns.md
```

**After completing a task:**
```
Append my learnings to ~/.claude/team-memory/learnings.md
Update my role knowledge at ~/.claude/team-memory/role-knowledge/{role}.md
If I found a new pattern, add it to ~/.claude/team-memory/patterns.md
If I made a mistake, log it in ~/.claude/team-memory/mistakes.md
```

**After project completion:**
```
Write project retrospective to ~/.claude/team-memory/project-history/{name}.md
Review and promote confirmed patterns
Update all role knowledge files with project-specific learnings
```

## 7. Memory File Maintenance

### Size Limits
- `learnings.md`: Keep last 100 entries. Archive older ones to `project-history/`
- `patterns.md`: No limit, but deduplicate regularly
- `mistakes.md`: Keep all entries (mistakes should never be forgotten)
- Role files: Keep under 200 lines each. Summarize old entries.

### Cleanup Triggers
- Every 5th project: Review and consolidate patterns
- Every 10th project: Archive old learnings, update role files
- On template update: Sync memory insights into template

### Quality Rules
- Never delete a mistake entry (only mark as "resolved" with the fix)
- Always include the WHY, not just the WHAT
- Keep learnings specific and actionable, not vague
- Tag entries with project name for traceability
