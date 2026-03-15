You are planning a phase of work and creating implementation tickets. The planning ticket: $ARGUMENTS

## Philosophy

Phase planning tickets exist to enable **just-in-time planning**. By the time you reach this phase, earlier work has been completed and you have real knowledge to plan with. The project overview has been updated with learnings. Now you can create detailed implementation tickets that reflect the current reality.

**Your output is tickets, not code.**

## Step 0: Normalize Ticket ID

Before fetching, normalize the input to a valid Linear ticket ID:

1. **Uppercase it** - "abc-11" → "ABC-11"
2. **Add hyphen if missing** - "abc11" → "ABC-11" (split where letters meet numbers)
3. **Infer team prefix if just a number** - "11" → check git history for recent ticket prefixes (e.g., `git log --oneline -20` often shows "ABC-123" patterns), then use that prefix

## Step 1: Load Context

1. **Fetch the ticket** from Linear - extract:
   - Title
   - Description
   - Project it belongs to

### Verify This Is a Planning Ticket

**If the ticket title does NOT match the pattern `[Phase]: [Letter]-1 Plan...`**, this is an implementation ticket, not a planning ticket.

Stop and tell the user:

> This is an implementation ticket, not a planning ticket.
>
> Run `/start-ticket [ticket-id]` instead to implement this ticket.
>
> `/plan-phase` is for planning tickets like `Backend: B-1 Plan API endpoints...` which produce implementation tickets.

Do NOT proceed with the rest of this workflow for implementation tickets.

### Extract Planning Context

From the planning ticket:
   - Title (should follow `[Phase]: [Letter]-1 Plan...` format)
   - Description (any guidance on what to analyze)
   - Project it belongs to

2. **Fetch the project overview** using get_project:
   - Current objectives and goals
   - Full scope (what's included and NOT included)
   - Architecture & approach
   - Open questions
   - Any updates from previous phases

3. **Check completed work** - look at other tickets in this project:
   - What phases are already done?
   - What did we learn? (check ticket descriptions, comments)
   - Did requirements change from the original plan?

## Step 2: Identify the Phase

Extract from the ticket title:
- **Phase name** (e.g., "Backend", "Frontend", "Testing")
- **Phase letter** (e.g., "B", "F", "T")

This determines the naming convention for tickets you'll create: `[Phase]: [Letter]-2 ...`, `[Phase]: [Letter]-3 ...`, etc.

## Step 3: Explore and Analyze

Based on the phase and project overview, explore what's needed:

1. **For Backend phases:**
   - What API endpoints are needed?
   - What services/modules need to be created or modified?
   - What data flows need to be implemented?
   - What integrations are required?

2. **For Frontend phases:**
   - What screens/pages are needed?
   - What components need to be built?
   - What state management is required?
   - What API integrations are needed?

3. **For Testing phases:**
   - What needs unit tests?
   - What needs integration tests?
   - What needs E2E tests?
   - What edge cases should be covered?

4. **For other phases:**
   - Break down the phase into logical, focused chunks of work
   - Each chunk should be completable in a focused session

**Explore the codebase** to understand:
- Existing patterns we should follow
- Files/modules that will be affected
- Dependencies between pieces of work

## Step 4: Draft Implementation Tickets

Create a list of implementation tickets for this phase. Each ticket should:

**Follow the naming convention:**
```
[Phase]: [Letter]-[Number] [Action verb] [Specific outcome]
```

**Be properly scoped:**
- Completable in a focused session (not multi-day epics)
- Has clear boundaries (what's included, what's not)
- Can be worked on somewhat independently (minimize blocking dependencies)

**Include enough detail:**
- Context: Reference the project overview, explain why this ticket matters
- Goal: What specific outcome are we achieving?
- Acceptance Criteria: How do we know it's done?
- Technical Notes: Files involved, approach hints, dependencies

**Order matters:**
- Number tickets in logical order (dependencies first)
- If B-2 must be done before B-3, make that clear

## Step 5: Present the Plan

Show the user:

### Phase: [Phase Name]
**Planning Ticket:** [ID - Title]
**Project:** [Project Name]

### Context from Project Overview
[Relevant excerpts - current scope, architecture decisions, any changes from original plan]

### Learnings from Previous Phases
[What did we discover? What changed? How does it affect this phase?]

### Proposed Tickets

**[Phase]: [Letter]-2 [Title]**
- **Why:** [How this enables other work or delivers value]
- **Scope:** [What's included]
- **Acceptance Criteria:** [Key "done" conditions]
- **Technical Notes:** [Files, approach, dependencies]

**[Phase]: [Letter]-3 [Title]**
- **Why:** [...]
- **Scope:** [...]
- **Acceptance Criteria:** [...]
- **Technical Notes:** [...]

[Continue for all tickets...]

### Dependencies
- [Letter]-2 must be done before [Letter]-4 because...
- [Letter]-3 and [Letter]-5 can be done in parallel

---

**Use AskUserQuestion to gather decisions and confirm:**

1. Ask any open questions about scope, approach, or priorities (with selectable options)
2. Final question: "Does this phase breakdown look good?"
   - Options: "Yes, create the tickets", "Needs adjustments"

## Step 6: Create Tickets and Complete Planning

After user confirmation:

1. **Create each implementation ticket** in Linear:
   - Use the full naming convention in the title
   - Include Context, Goal, Acceptance Criteria, Technical Notes in description
   - Add to the same project
   - **Set status to "Todo"** (not Triage - these are ready to work)
   - Set appropriate dependencies/blocking relationships if Linear supports it

2. **Mark the planning ticket as Done** - its output (the implementation tickets) has been delivered

3. **Update the project overview if needed** - if this planning surfaced new information, decisions, or scope changes, update the project description to reflect current state

4. **Present summary:**
   - List of created tickets with IDs
   - Any updates made to project overview
   - Suggested next ticket to start

---

## If Requirements Changed

If during this planning you discover the project overview is outdated:

1. **Surface it explicitly:** "The project overview says X, but based on [previous work / new understanding], it should be Y"
2. **Propose the specific change** - what section, what text
3. **Get user confirmation** before updating
4. **Make a surgical update:**
   - Read the current project overview first
   - Only modify the specific section that needs updating
   - Preserve all other content exactly as-is
   - Add to existing sections rather than replacing them when possible
5. **Then proceed** with ticket creation based on the updated scope

This keeps the project overview as the source of truth.
