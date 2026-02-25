# Phase Discussion Workflow

This runs BEFORE planning each phase. It identifies gray areas — decisions that could go multiple ways — and locks them down so developers don't guess.

## Purpose

Extract implementation decisions that downstream agents need. The user is the visionary — the team is the builder. This captures HOW things should work so the PM, Tech Lead, and devs have clear direction.

## Who Runs This

The **relevant domain roles** for the phase lead the discussion:

| Phase Type | Discussion Led By |
|---|---|
| UI/Frontend phase | UX + Frontend Dev |
| API/Backend phase | Tech Lead + Backend Dev |
| Data/Schema phase | Tech Lead + Data Engineer |
| Full-stack feature | PM + Tech Lead + UX (tag-team) |
| Infrastructure phase | Tech Lead + DevOps |

**Devil's Advocate observes** and notes concerns silently (saves for later review).

## When This Runs

- Before planning EACH phase (not just once at project start)
- After the PROJECT-BRIEF.md exists and PRD is written
- Time-box: 15-30 minutes per phase

---

## The Discussion Process

### Step 1: Identify Gray Areas

Read the phase goal from the PRD/roadmap and determine:

1. **Domain boundary** — What capability is this phase delivering? State it clearly.
2. **Gray areas** — What decisions would change the outcome that the user should weigh in on?
3. **Skip assessment** — If no meaningful gray areas exist (pure infrastructure, clear-cut implementation), skip discussion for this phase.

Gray areas are **implementation decisions the user cares about** — things that could go multiple ways and would change the result.

**How to identify them by phase type:**

- Something users **SEE** → layout, visual style, interactions, states, density
- Something users **CALL** (API) → response format, error handling, contract details
- Something users **RUN** (CLI/tool) → invocation style, output format, behavior modes
- Something users **READ** (content) → structure, tone, depth, flow
- Something being **ORGANIZED** → criteria, grouping, exceptions, edge cases

### Step 2: Present Gray Areas

State the phase boundary first (prevents scope creep):

```
Phase [X]: [Name]
Domain: [What this phase delivers]

We'll clarify HOW to implement this.
New capabilities belong in other phases.
```

Then present 3-5 phase-specific gray areas for the user to select:

```
Which areas do you want to discuss for [phase name]? (pick any)

[ ] Layout style — Cards vs list vs timeline? Information density?
[ ] Loading behavior — Infinite scroll or pagination? Pull to refresh?
[ ] Content ordering — Chronological, algorithmic, or user choice?
[ ] Post metadata — What info per post? Timestamps, reactions, author?
```

**Generate specific gray areas, not generic categories.** Don't use labels like "UI", "UX", "Behavior" — use concrete decisions.

Examples by domain:

For "User Dashboard" (visual feature):
```
[ ] Layout approach — Widgets/cards vs single-page list? Customizable?
[ ] Key metrics — Which numbers matter most? What's above the fold?
[ ] Data freshness — Real-time updates or refresh-on-load?
[ ] Empty states — What shows before the user has any data?
```

For "Database backup CLI" (command-line tool):
```
[ ] Output format — JSON, table, or plain text? Verbosity levels?
[ ] Flag design — Short flags, long flags, or both? Required vs optional?
[ ] Progress reporting — Silent, progress bar, or verbose logging?
[ ] Error recovery — Fail fast, retry, or prompt for action?
```

For "REST API for orders" (backend):
```
[ ] Response shape — Nested objects or flat with IDs? Pagination style?
[ ] Error format — HTTP status only or structured error body?
[ ] Filtering — Query params, POST body, or GraphQL-style?
[ ] Rate limiting — Per user, per endpoint, or global?
```

### Step 3: Discuss Selected Areas

For each selected area:

1. **Announce:** "Let's talk about [Area]."

2. **Ask 3-4 focused questions** with concrete options:
   ```
   How should the dashboard layout work?

   1. Fixed grid of cards (like Vercel dashboard)
   2. Customizable widgets (like Notion)
   3. Single scrollable page with sections
   4. Something else — describe it

   Recommendation: Option 1 — simpler to build, proven pattern,
   customization can come in v2.
   ```

3. **After 3-4 questions, check:**
   "More questions about [area], or move to the next one?"

4. **Capture "You decide" responses** — when the user says "whatever you think" or "your call", log it as "Claude's Discretion" in the output. This means the dev has flexibility.

### Step 4: Scope Creep Guard

**Discussion clarifies HOW to implement what's scoped. It never adds new capabilities.**

Allowed (clarifying ambiguity):
- "How should posts be displayed?" (layout decision)
- "What happens on empty state?" (within the feature)
- "Pull to refresh or manual?" (behavior choice)

Not allowed (scope creep):
- "Should we also add comments?" (new capability)
- "What about search/filtering?" (new capability)
- "Maybe include bookmarking?" (new capability)

When user suggests scope creep:
```
"[Feature X] would be a new capability — that's its own phase.
I'll note it as a deferred idea so we don't lose it.

For now, let's focus on [current phase domain]."
```

Track deferred ideas. Don't lose them, don't act on them.

### Step 5: Check for Remaining Unknowns

After all selected areas are discussed:

```
We've covered [list areas]. Anything else unclear about this phase,
or are we good to lock these decisions?

1. Explore more gray areas — I want to discuss [specific thing]
2. Lock it in — these decisions are good, let's move to planning
```

If "Explore more" → identify 2-3 additional gray areas based on what was learned, discuss those, then ask again.

---

## Output: PHASE-CONTEXT.md

After discussion, write this to `~/.claude/project-briefs/{project-name}/phase-{N}-context.md`

```markdown
# Phase [X]: [Name] - Context

**Gathered:** [date]
**Discussion led by:** [roles involved]
**Status:** Ready for planning

## Phase Boundary

[Clear statement of what this phase delivers — the scope anchor.
Anything outside this boundary belongs in another phase.]

## Implementation Decisions

### [Area 1 that was discussed]
- [Decision or preference captured]
- [Another decision if applicable]

### [Area 2 that was discussed]
- [Decision or preference captured]

### [Area N]
- [Decisions...]

### Claude's Discretion
[Areas where the user said "you decide" or "your call".
Developers have flexibility here but should follow team patterns.]

- [Area where dev can choose approach]
- [Area where dev can choose approach]

## Specific References

[Any "I want it like X" moments, reference apps, screenshots, or
specific behaviors the user described.]

If none: "No specific references — open to standard approaches."

## Deferred Ideas

[Ideas that came up during discussion but belong in other phases.
PM should review these when planning the roadmap.]

- [Deferred idea 1] — suggested during [area] discussion
- [Deferred idea 2] — suggested during [area] discussion

If none: "Discussion stayed within phase scope."

---
*Phase context gathered: [date]*
*Ready for: Planning and development of Phase [X]*
```

---

## What Happens After Discussion

```
PHASE-CONTEXT.md created
    ↓
PM + Tech Lead plan the phase (informed by context — no guessing)
    ↓
Devs build (refer to PHASE-CONTEXT.md for decisions)
    ↓
QA validates (checks that decisions were followed)
```

Every developer working on this phase reads PHASE-CONTEXT.md before starting. If they encounter a decision not covered, they escalate to PM/Tech Lead rather than guessing.

---

## Downstream: Who Reads This

| Role | What They Use From PHASE-CONTEXT.md |
|---|---|
| PM | Decisions to incorporate into detailed task specs |
| Tech Lead | Locked decisions to respect in architecture choices |
| Frontend Dev | UI decisions, layout choices, interaction patterns |
| Backend Dev | API shape decisions, error handling approach |
| Data Engineer | Schema decisions, data model choices |
| QA Lead | Decisions to validate against (did devs follow them?) |
| Devil's Advocate | Decisions to challenge if they spot issues during review |

---

## When To Skip Discussion

Not every phase needs discussion. Skip when:

- **Pure infrastructure** — "Set up CI/CD pipeline" has no user-facing gray areas
- **Clear-cut from PRD** — The PRD already specifies exactly how it should work
- **Internal refactor** — "Migrate from REST to GraphQL" is a technical decision, not a user decision

When skipping, note it:
```
Phase [X]: [Name] — Discussion skipped (infrastructure/clear-cut).
Proceeding directly to planning.
```

---

## Discussion Checklist

- [ ] Phase boundary stated clearly (scope anchor)
- [ ] Gray areas identified (specific to this phase, not generic)
- [ ] User selected which areas to discuss
- [ ] Each area explored with concrete options and recommendations
- [ ] Scope creep redirected to deferred ideas
- [ ] "Claude's Discretion" areas captured (where dev has flexibility)
- [ ] Specific references captured (if any)
- [ ] PHASE-CONTEXT.md written
- [ ] User confirmed decisions are locked
