You are helping create a well-structured Linear ticket. The user's rough idea: $ARGUMENTS

## Step 1: Detect Context

1. **Fetch available teams** from Linear using the list_teams tool
2. **Check the current working directory** and git remote to infer which team this relates to
3. If you can confidently match the repo/folder to a team, use that team
4. If unclear, **use AskUserQuestion** to let the user pick from the available teams

Once a team is selected, **fetch that team's projects** from Linear using list_projects - don't hardcode project names.

## Step 1b: Load Project Context (if specified)

**Only do this step if the user explicitly mentions a project** (e.g., "ticket for the Auth project", "this is for Onboarding", "add to Dashboard redesign").

1. **Fuzzy match the project name** - The user may not spell it exactly right or use the exact wording. Search the projects list for:
   - Case-insensitive matches
   - Partial matches (e.g., "auth" matches "Authentication Overhaul")
   - Word matches (e.g., "dashboard" matches "Dashboard Redesign")

2. If multiple projects could match, **use AskUserQuestion** to let them pick

3. **Fetch the full project details** using get_project to get:
   - Project description and objectives
   - Current state and progress
   - Any documented scope or goals

4. **Use this context** when drafting the ticket to ensure it aligns with the project's goals and uses consistent terminology

## Step 2: Assess Scope

Evaluate if this is ticket-sized or needs to be broken up:

**Single ticket signals:**
- Single feature, fix, or enhancement
- Action words like "Fix", "Add", "Update", "Change", "Remove"
- Specific UI element, endpoint, or behavior
- Can be completed in a focused session

**Needs breakdown signals:**
- Touches 3+ distinct areas (database + backend + frontend, etc.)
- Would take multiple focused sessions
- Has natural phases that could be done independently
- Broad goals like "I want to...", "We need to build..."

**If it needs breakdown, use AskUserQuestion with context-aware options:**

If user specified a project (from Step 1b):
- "Single ticket" → Proceed with one ticket in that project
- "Multiple phase tickets" → Create 2-4 tickets in the same project
- "Too big - needs its own project" → Redirect to `/project-kickoff`

If no project specified:
- "Single ticket" → Create one standalone ticket
- "A few tickets" → Create 2-4 related tickets without a project wrapper
- "Full project" → Redirect to `/project-kickoff`

If they choose multiple tickets, gather enough context to create all of them with proper ordering (1. Database, 2. Backend, etc.).

## Step 3: Gather Details (for tickets)

Before drafting, clarify key decisions. **Use AskUserQuestion with selectable options whenever possible** - this is faster than open-ended questions.

Good candidates for selectable options:
- **Scope choices**: Which components/features are affected? (multi-select from what you find in the codebase)
- **UI/UX approach**: When there are 2-4 distinct implementation patterns
- **Behavior choices**: Live vs batch, sync vs async, strict vs lenient, etc.
- **Priority signals**: Is this blocking something? Nice-to-have?

Reserve open-ended questions only for things that truly need free-form answers (e.g., "What problem does this solve?" or context you can't infer).

Aim to gather:
1. What problem this solves / value it adds
2. What "done" looks like (acceptance criteria)
3. Key technical decisions

## Step 4: Suggest Labels

Based on the ticket context, suggest appropriate label(s):

| Label | When to suggest |
|-------|-----------------|
| **Bug** | Something is broken, not working as expected, or needs fixing |
| **Feature** | Net-new functionality or capability |
| **Improvement** | Enhancing existing functionality |
| **Quick Win** | Small, focused tasks that can be done quickly |
| **UX/UI** | Design, interface, or user experience work |

Labels can be combined (e.g., "Quick Win" + "Bug" for a simple fix). Suggest what seems right and confirm with the user.

## Step 5: Draft the Ticket

Based on the conversation, draft:

**Title:** Clear, actionable, starts with a verb (Fix, Add, Implement, Update, etc.)

**Description:** Structured as:
```
## Context
[Why this matters / background]

## Goal
[What we're trying to achieve]

## Acceptance Criteria
- [ ] [Specific, testable criteria]
- [ ] [Another criterion]

## Technical Notes (if relevant)
[Any implementation hints, dependencies, or constraints]
```

## Step 6: Confirm and Create

Show the user:
- Team: [detected team]
- Project: [suggested project]
- Labels: [suggested labels]
- Title: [drafted title]
- Description: [drafted description]

Ask: "Does this look good? Any changes before I create it?"

Only create the ticket in Linear after user confirmation.

After creating, just confirm with the ticket ID and URL. Don't display the git branch name.
