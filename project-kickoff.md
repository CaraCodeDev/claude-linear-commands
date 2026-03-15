You are helping create a well-structured Linear Project. The user's idea: $ARGUMENTS

## Philosophy

The **project overview is the source of truth**. When work begins on any ticket, the first thing that happens is reading the project overview to understand where that ticket fits in the larger vision. A weak overview leads to disjointed tickets and lost context.

**Phase tickets are the work tickets.** Each phase (Backend, Frontend, Database, etc.) gets one ticket that's directly implementable. The project overview provides enough context - we don't need to break phases into many sub-tickets. This keeps administrative overhead low and lets you ship faster.

## Step 1: Detect Context

1. **Fetch available teams** from Linear using the list_teams tool
2. **Check the current working directory** and git remote to infer which team this belongs to
3. If you can confidently match the repo/folder to a team, use that team
4. If unclear, **use AskUserQuestion** to let the user pick from the available teams

## Step 2: Brainstorm the Initiative

**This is a conversation, not a checklist.** The user often starts with a rough one-liner. Your job is to explore and flesh it out together through back-and-forth discussion.

**Start by exploring:**
- What problem are we solving? Why does it matter?
- Who benefits and how?
- What does success look like?

**Explore the codebase as you go:**
- What exists already that's relevant to this initiative?
- What patterns, conventions, or architecture should we follow?
- What would we be building on or integrating with?
- Are there similar features we can learn from?

Share what you find - it informs the conversation. "I see you have X already, would this build on that?" or "The current auth system uses Y pattern, should we follow that?"

**Then dig deeper as needed:**
- What are the major pieces of work?
- What constraints or dependencies should we know about?
- What's explicitly out of scope?

**Use AskUserQuestion for decisions, not discovery.** When you need to understand something, just ask in plain text and let the conversation flow. Use AskUserQuestion when there are concrete choices to make (e.g., "Which approach: A or B?").

**Keep going until you both agree the idea is fleshed out.** Don't rush to create phases. Some projects need 2 exchanges, some need 10. Signs you're ready:
- The problem and solution are clear
- You understand the existing codebase context
- You can articulate what's in scope and out of scope
- You have a sense of the major chunks of work
- The user feels like their vision has been captured

Only proceed to Step 3 when the brainstorming feels complete.

## Step 3: Draft the Project Overview

The project overview is the north star document. Someone reading this 3 weeks from now should fully understand the vision, scope, and current thinking - even if they haven't seen any tickets.

**Name:** Clear, descriptive name for the initiative

**Summary:** One-line description (max 255 chars) - what this project delivers

**Description:** This must be comprehensive. Structure as:

```markdown
## Overview

[2-3 paragraphs that explain the full vision. What problem are we solving? Why does it matter? What does success look like? Write this as if explaining to a new team member who needs to understand the entire initiative.]

## Goals

- [Specific, measurable goal 1]
- [Specific, measurable goal 2]
- [Continue as needed - be concrete, not vague]

## Scope

### What's Included
[Explicit list of what this project covers. Be specific about features, behaviors, and boundaries.]

### What's NOT Included
[Equally important - what are we deliberately not doing? This prevents scope creep and clarifies boundaries.]

## Architecture & Approach

[How will this be built? What patterns will we follow? What are the key technical decisions? Include enough detail that someone starting a ticket understands the overall shape of the solution.]

- Key components/modules involved
- Data flow or state management approach
- Integration points with existing systems
- Any new patterns or conventions being introduced

## Dependencies & Constraints

- [External dependencies - other projects, APIs, services]
- [Internal dependencies - existing features this builds on]
- [Constraints - timeline pressures, technical limitations, compatibility requirements]

## Open Questions

[What's still uncertain? What needs to be figured out as we go? Being explicit about unknowns prevents false confidence.]

## Success Criteria

[How do we know this project is complete? What can we demonstrate or measure?]
```

**Important:** The overview should contain ALL the context needed to understand this project. Tickets will reference this document, not duplicate its content.

## Step 4: Identify Phases and Create Phase Tickets

Break the project into logical phases. **Scale to the project size** - don't over-ticket:

| Project Size | Typical Tickets | Example |
|--------------|-----------------|---------|
| Small (focused feature) | 1-2 | Just "Implement X" or "Backend + Frontend" |
| Medium (multi-part feature) | 2-4 | Database, API, UI |
| Large (system/initiative) | 4-8 | Schema, Backend services, API layer, Frontend components, Integration, Testing |

**Don't force phases that don't exist.** If there's no database work, don't create a database ticket. If it's a backend-only change, one ticket is fine.

**Every project needs testing and QA tickets.** Automated testing and manual QA are first-class work, not afterthoughts. For each major feature or phase, create a dedicated ticket covering automated tests (e.g., `5. Tests: Unit and integration tests for permissions system`).

**Docs tickets are only for user-facing documentation.** "Docs" means content for the people who *use* the app — help articles, admin guides, onboarding walkthroughs, API docs for external consumers. Do NOT create docs tickets for internal developer documentation — that belongs in code comments, the project overview, and inline documentation written during implementation. Only add a docs ticket if the feature genuinely needs user-facing or admin-facing documentation. Many projects don't. When a docs ticket is created, note in the description that `/create-doc` should be used to execute it — it handles doc type, file placement, tone, and screenshot placeholders.

**Every project also needs a QA ticket.** This is a human verification ticket — a checklist of things to manually test by logging in and clicking around. It should be assignable to team members who weren't involved in building the feature. Structure it as:
- A clear checklist of specific behaviors to verify (not vague — "click X, expect Y")
- Happy path scenarios and edge cases
- Different user roles or permission levels to test as, if applicable
- Any setup instructions needed (test accounts, seed data, feature flags)

The QA ticket should be the **last ticket** in the phase order, since it can only be done after everything else is built. Name it like: `6. QA: Manual verification of [feature]`.

### Ticket Naming Convention

```
[Order]. [Phase]: [Action verb] [what gets built]
```

Examples for a medium project:
- `1. Backend: Implement user permissions API`
- `2. Frontend: Build permissions management UI`

Examples for a large project:
- `1. Database: Create permissions schema and migrations`
- `2. Backend: Implement role and permission services`
- `3. API: Add permission endpoints and middleware`
- `4. Frontend: Build admin permissions UI`
- `5. Integration: Connect auth system to new permissions`
- `6. Tests: Unit and integration tests for permissions system`
- `7. QA: Manual verification of permissions system`

The number prefix indicates recommended order (dependencies first).

### When to Use Sub-Tickets

Some phases are too large for a single ticket. **Use sub-tickets when a phase has:**
- Multiple distinct functional areas (e.g., auth pages vs dashboard vs settings)
- Multiple route groups or page clusters
- Work that naturally splits into 3+ focused sessions

**Sub-ticket naming uses letter suffixes:**
```
[Order][Letter]. [Phase]: [Specific area]
```

Example - Frontend phase with sub-tickets:
- `2. Frontend: Customer dashboard UI` (parent - holds overall scope)
  - `2a. Frontend: Build auth pages` (child)
  - `2b. Frontend: Build dashboard layout and project list` (child)
  - `2c. Frontend: Build project detail and quote views` (child)
  - `2d. Frontend: Build account settings` (child)

**The parent ticket contains:**
- High-level scope for the entire phase
- Shared context (design patterns, component conventions, integration points)
- The full picture of what "Frontend done" means

**Each sub-ticket contains:**
- Specific scope for that chunk
- Key components/files involved
- Acceptance criteria for that sub-ticket
- Reference to parent for broader context

**Don't over-split.** Most phases don't need sub-tickets. Only split when you'd otherwise be creating a ticket with 4+ distinct areas that would take multiple focused sessions to complete.

### Standard Phase Tickets

**For each phase ticket, provide:**
- **Title:** Following the naming convention
- **Description:** Include enough context that `/start-ticket` can execute it:
  - What this phase delivers (reference project overview sections)
  - Key components or files likely involved
  - Any constraints or patterns to follow
  - Acceptance criteria for the phase

**Keep it high-level.** The project overview has the detailed context. The phase ticket scopes what "done" looks like for that chunk of work.

## Step 5: Confirm and Create

Show the user:
- Team: [detected team]
- Project Name: [name]
- Summary: [summary]
- Description: [full description]
- Phase Tickets: [list in order with what each delivers]

**Use AskUserQuestion:** "Does this look good?"
- "Yes, create the project" → proceed to create
- "No, needs changes" → ask what to change, update, then re-confirm

After confirmation:
1. Create the project in Linear
2. Use AskUserQuestion: "Create the initial tickets now?" → Yes, create them / No, just the project
