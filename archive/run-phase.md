You are orchestrating autonomous execution of a phase of tickets. The phase/project: $ARGUMENTS

## Philosophy

This is **autopilot mode**. You orchestrate ticket execution without asking questions. Each ticket's implementation runs in a **separate Task agent** for context isolation. When facing ambiguous decisions, agents choose the best option based on clear priorities and document their reasoning. The user reviews everything at the end.

**Decision Priority (agents follow this order):**
1. **Consistency with existing patterns** - Match what's already in the codebase
2. **Simplicity** - Prefer straightforward solutions over clever ones
3. **Less scope** - Do the minimum needed, don't expand scope

## Step 0: Normalize Input

The input is a **planning ticket ID** - the same ticket you ran `/plan-phase` on.

Example: `/run-phase ABC-11` where ABC-11 is "Backend: B-1 Plan API endpoints..."

Normalize the ticket ID:
1. **Uppercase it** - "abc-11" → "ABC-11"
2. **Add hyphen if missing** - "abc11" → "ABC-11"
3. **Infer team prefix if just a number** - check git history for recent ticket prefixes

## Step 1: Load Phase Context

1. **Fetch the planning ticket** to extract:
   - Phase name and letter (e.g., "Backend", "B")
   - Project it belongs to

2. **Find the implementation tickets** created by `/plan-phase`:
   - Use `list_issues` with:
     - `project`: the project ID
     - `state`: "Todo" (implementation tickets are created in Todo status)
     - `query`: the phase prefix (e.g., "Backend: B-") to filter by title
     - `limit`: 20 (phases shouldn't have more than this)
   - From results, filter to ticket number > 1 (exclude B-1 planning ticket)
   - Order by ticket number (B-2, B-3, B-4...)

3. **Fetch the project overview** if available:
   - Objectives and goals
   - Architecture & approach
   - Scope boundaries

3. **Present the execution plan:**

```
## Phase Execution Plan

**Project:** [Name]
**Tickets to execute:** [count]

1. [ABC-12] Backend: B-2 Implement user service
2. [ABC-13] Backend: B-3 Add authentication endpoints
3. [ABC-14] Backend: B-4 Create role permissions
...

Starting autonomous execution. Each ticket runs in a separate agent for fresh context.
```

## Step 2: Execute Each Ticket

For each ticket in order:

### 2a. Orchestrator: Prepare the Ticket

1. **Update status** to "In Progress" via Linear API
2. **Fetch full ticket details** - description, acceptance criteria, technical notes
3. **Load any comments** - previous context or decisions

### 2b. Spawn Task Agent for Implementation

Use the **Task tool** with `subagent_type: "general-purpose"` to execute the ticket.

The prompt should include:
- Full ticket details (ID, title, description, acceptance criteria, technical notes)
- Project context summary (objectives, architecture, scope)
- The decision priority hierarchy
- Clear instruction to NOT ask questions - decide and document
- Request for structured JSON summary at the end

Example Task prompt structure:
```
Implement ticket [ID]: [Title] autonomously. Do NOT ask questions.

TICKET:
[Full description, acceptance criteria, technical notes]

PROJECT CONTEXT:
[Objectives, architecture, scope boundaries]

DECISION PRIORITY (follow this order):
1. Consistency with existing patterns
2. Simplicity
3. Less scope

INSTRUCTIONS:
1. Explore the codebase to understand where this work lives
2. Implement the requirements, following existing patterns
3. When choosing between options, pick based on decision priority and document why
4. Return a summary with: status (completed/blocked), what was done, files changed, decisions made, any blocker reason

Do the work. Write the code. Decide autonomously and document your reasoning.
```

### 2c. Orchestrator: Process Agent Result

When the Task agent returns:

1. **Parse the result** - extract status, summary, files changed, decisions

2. **Post detailed comment** to the ticket in Linear:

```markdown
## Completed

[Summary bullets from agent]

## Changes

[Files changed from agent]

## Decisions Made

[For each decision:]
- **[Decision]**: [rationale]

## Testing

[How it was verified]
```

3. **Update ticket status:**
   - If completed → set to "Done"
   - If blocked → leave "In Progress", blocker is documented in comment

4. **Report progress to user:**
   ```
   ✓ [ABC-12] Backend: B-2 Implement user service - Done
     └─ Added UserService, created repository pattern
   → Starting [ABC-13] Backend: B-3 Add authentication endpoints...
   ```

5. **Track for final summary:**
   - Store ticket result (summary, decisions, files, status)

### 2d. Handle Blockers

If agent reports blocked status:

1. **Do not stop the entire phase**
2. **Comment already posted** with blocker details
3. **Track for end summary**
4. **Continue to next ticket**

## Step 3: Phase Complete - Review

After all tickets are processed, present:

```
## Phase Execution Complete

**Tickets Completed:** X of Y
**Tickets Blocked:** Z (if any)

### Summary by Ticket

#### ✓ [ABC-12] Backend: B-2 Implement user service
- Added UserService class with CRUD operations
- Created user repository with PostgreSQL adapter
- **Decision:** Used repository pattern (matches existing OrderRepository)

#### ✓ [ABC-13] Backend: B-3 Add authentication endpoints
- Implemented /login, /logout, /refresh endpoints
- Added JWT token generation
- **Decision:** 15-minute token expiry (simpler than refresh token rotation)

#### ⚠ [ABC-14] Backend: B-4 Create role permissions
- **Blocked:** Needs schema migration that conflicts with pending PR #42
- Left in progress with blocker documented

### Decisions Log

| Ticket | Decision | Rationale |
|--------|----------|-----------|
| B-2 | Repository pattern | Matches existing OrderRepository |
| B-3 | 15-min token expiry | Simpler than rotation |

### Files Changed

[git status summary]

---

**Ready for your review.** Take a look at the changes and decisions above.
```

**Use AskUserQuestion:**
- Question: "How would you like to proceed?"
- Options:
  - "Commit the phase" - Create a single commit for all phase work
  - "Review changes first" - Let me look at specific files/decisions
  - "Revert and discuss" - Something needs to change

## Step 4: Finalize

Based on user choice:

### If "Commit the phase":

1. **Stage all changes:** `git add .`

2. **Create commit:**
```
[Phase]: Complete [phase name] implementation

Tickets completed:
- [ABC-12] B-2 Implement user service
- [ABC-13] B-3 Add authentication endpoints

Key decisions:
- Used repository pattern for data access (consistency)
- 15-minute JWT expiry (simplicity)

🤖 Generated with Claude Code
```

3. **Report back:**
   - Commit hash
   - Link to project in Linear
   - Any blocked tickets that need attention

### If "Review changes first":

Walk through specific files or decisions as requested. Then return to the commit question.

### If "Revert and discuss":

Do NOT commit. Discuss what needs to change. May need to re-run individual tickets with `/start-ticket` for user-guided implementation.

---

## Error Recovery

- **Agent fails or times out:** Note in ticket comment, mark as blocked, continue to next ticket
- **Build/test failures:** Agent should attempt fix; if stuck, return blocked status
- **Conflicting patterns found:** Agent chooses the more recent pattern, documents the inconsistency
