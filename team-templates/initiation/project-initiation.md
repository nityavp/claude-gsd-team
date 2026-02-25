# Project Initiation Workflow

This is Phase -1. It runs BEFORE anything else. No PRD, no design, no architecture — nothing starts until initiation is complete.

## Purpose

Extract the user's vision through collaborative questioning. Produce a PROJECT-BRIEF.md that gives the PM everything needed to write a focused PRD without guessing.

## Who Runs This

**CEO** leads the questioning (strategic: why, who, what success looks like)
**PM** assists (product: features, user journeys, scope boundaries)
**Devil's Advocate** observes silently, notes concerns for later

## When This Runs

- Every new project, no exceptions
- Before memory read, before research, before anything
- Time-box: 30-60 minutes depending on project complexity

---

## The Questioning Process

### Philosophy

You are a thinking partner, not an interviewer. The user has a fuzzy idea — your job is to sharpen it. Ask questions that make them think "oh, I hadn't considered that" or "yes, that's exactly what I mean."

Don't interrogate. Collaborate. Don't follow a script. Follow the thread.

### What You Need By The End

Four things. If the user volunteers more, capture it — but these four are non-negotiable:

- [ ] **What** they're building (concrete enough to explain to a stranger)
- [ ] **Why** it needs to exist (the problem or desire driving it)
- [ ] **Who** it's for (even if just themselves)
- [ ] **What "done" looks like** (observable outcomes, not vague goals)

### How To Question

**Start open.** Let them dump their mental model. Don't interrupt with structure.

```
"Tell me about this project. What are you thinking?"
```

**Follow energy.** Whatever they emphasized, dig into that. What excited them? What problem sparked this?

**Challenge vagueness.** Never accept fuzzy answers:
- "Good UX" → "Good how? Fast? Simple? Familiar?"
- "Users" → "Which users? How many? What do they do today?"
- "Simple" → "Simple to build or simple to use? What's the simplest version?"
- "Scalable" → "Scalable to what? 100 users? 100k? What breaks first?"

**Make the abstract concrete:**
- "Walk me through using this."
- "What does that actually look like on screen?"
- "Give me an example of a real person using this."

**Clarify ambiguity:**
- "When you say X, do you mean A or B?"
- "You mentioned Y — tell me more about that."

**Know when to stop.** When you understand what, why, who, and done — offer to proceed.

### Question Types (Inspiration, Not Checklist)

**Motivation — why this exists:**
- "What prompted this?"
- "What are you doing today that this replaces?"
- "What's the cost of NOT building this?"

**Concreteness — what it actually is:**
- "Walk me through using this end-to-end."
- "What's the first thing a user sees?"
- "What's the most important action a user takes?"

**Clarification — what they mean:**
- "When you say Z, do you mean A or B?"
- "You mentioned X — is that for v1 or later?"

**Boundaries — what it's NOT:**
- "What's explicitly NOT part of this?"
- "If you had to cut one feature, which goes?"
- "What's the smallest version that's still useful?"

**Success — how you'll know it works:**
- "How will you know this is working?"
- "What does v1 success look like? Be specific."
- "Who's the first person you'd show this to?"

**Existing landscape:**
- "What do you use today for this?"
- "What's the closest thing that already exists? What's wrong with it?"
- "Have you tried [obvious alternative]? Why didn't it work?"

### Presenting Options (Use When Clarifying)

When the user gives a vague answer, present concrete options to react to:

```
User says: "It should be fast"

Ask: "Fast how?"
Options:
  1. Sub-second response times (performance)
  2. Quick to set up and start using (onboarding)
  3. Fast to build — MVP in days not weeks (scope)
  4. Something else — tell me
```

Good options are:
- Concrete interpretations of what they might mean
- Specific examples to confirm or deny
- 2-4 choices (not more)

Bad options are:
- Generic categories ("Technical", "Business")
- Leading options that presume an answer
- More than 4 choices

### Decision Gate

When you have enough to write a clear PROJECT-BRIEF.md, offer to proceed:

```
"I think I have a solid picture of what you're building. Ready to lock this in as the project brief?

1. Create PROJECT-BRIEF.md — let's move forward
2. Keep exploring — I want to share more or dig deeper
```

If "Keep exploring" — ask what they want to add, or identify gaps and probe naturally.

Loop until the user confirms they're ready.

### Anti-Patterns (NEVER Do These)

- **Checklist walking** — Going through categories regardless of what they said
- **Corporate speak** — "What are your success criteria?" "Who are your stakeholders?"
- **Interrogation** — Firing questions without building on answers
- **Rushing** — Minimizing questions to "get to the work"
- **Shallow acceptance** — Taking vague answers at face value
- **Premature tech talk** — Asking about stack before understanding the idea
- **Asking about user's skill level** — The team builds. The user envisions.

---

## Output: PROJECT-BRIEF.md

After questioning, CEO + PM produce this document. It lives at `~/.claude/project-briefs/{project-name}.md` and feeds directly into PM's PRD.

```markdown
# Project Brief: [Project Name]

**Created:** [date]
**Gathered by:** CEO + PM during Project Initiation

## What This Is

[2-3 sentences. What does this product do and who is it for?
Use the user's exact language and framing.]

## Why It Exists

[The problem or desire driving this project.
What happens if this doesn't get built?
What's the user doing today instead?]

## Who It's For

[Primary users — be specific.
If there are multiple user types, list them with what each cares about.]

## Core Value

[The ONE thing that matters most. If everything else fails, this must work.
One sentence that drives prioritization when tradeoffs arise.]

## What "Done" Looks Like

[Observable outcomes for v1. Not features — outcomes.
"A user can ___" or "The system handles ___"]

- [Outcome 1]
- [Outcome 2]
- [Outcome 3]

## Key Features (User's Words)

[What the user described wanting. Keep their language.
Don't translate into technical requirements yet — that's PM's job in the PRD.]

- [Feature/capability 1]
- [Feature/capability 2]
- [Feature/capability 3]

## Explicitly NOT In Scope (v1)

[Things the user said no to, or that were discussed and deferred.
Include the reason — prevents scope creep later.]

- [Exclusion 1] — [why]
- [Exclusion 2] — [why]

## Existing Landscape

[What the user is doing today. What alternatives exist.
What's wrong with them. Why build instead of buy.]

## Constraints & Preferences

[Anything the user specified about how it should be built.
Tech stack preferences, deployment needs, timeline, budget, etc.
Only include what the user actually said — don't assume.]

## Open Questions

[Things that came up but weren't resolved.
PM should address these in the PRD or during Phase Discussion.]

- [Question 1]
- [Question 2]

## Raw Notes

[Key quotes or moments from the conversation.
Useful for PM to reference the user's original intent.]

---
*Initiation completed: [date]*
*Ready for: PM to write PRD (Phase 1)*
```

---

## What Happens After Initiation

```
PROJECT-BRIEF.md created
    ↓
Phase 0: ALL read memory + research (/last30days)
    ↓
Phase 0.1: DevOps creates GitHub repo + branches
    ↓
Phase 1: PM writes PRD (informed by PROJECT-BRIEF.md — no guessing)
    ↓
... rest of workflow continues as normal
```

The PROJECT-BRIEF.md becomes the PM's primary input. Every requirement in the PRD should trace back to something the user actually said during initiation. If PM needs to add something not in the brief, they flag it and confirm with CEO/user.

---

## Brownfield Projects (Existing Codebase)

If the user has an existing codebase:

1. **Ask first:** "Is this a new project or are we building on something that already exists?"
2. **If existing:** Have Tech Lead + Frontend scan the codebase BEFORE questioning
3. **During questioning:** Reference what already exists
   - "I see you have auth set up with Supabase. Are we keeping that?"
   - "The current UI uses shadcn/ui. Should we continue with that?"
4. **In PROJECT-BRIEF.md:** Add a "Current State" section documenting what exists

---

## Initiation Checklist (CEO Validates Before Moving On)

- [ ] User's vision is captured in their own words
- [ ] "Why" is clear — not just "what"
- [ ] Target users are identified (not "everyone")
- [ ] v1 scope has clear boundaries (in-scope AND out-of-scope)
- [ ] "Done" is defined in observable terms
- [ ] Existing alternatives were discussed
- [ ] Constraints and preferences are documented (only what user said)
- [ ] Open questions are logged for PM
- [ ] PROJECT-BRIEF.md is written and saved
- [ ] User has confirmed "yes, that captures what I want"
