You are reviewing a Linear ticket or project for quality and completeness: $ARGUMENTS

**IMPORTANT: This command is for ticket analysis and refinement only. The only thing you should update is the Linear ticket itself. DO NOT implement any code changes.**

## Step 0: Normalize Input

If the input looks like a ticket ID (contains numbers), normalize it first:

1. **Uppercase it** - "abc-11" → "ABC-11"
2. **Add hyphen if missing** - "abc11" → "ABC-11" (split where letters meet numbers)
3. **Infer team prefix if just a number** - "11" → check git history for recent ticket prefixes (e.g., `git log --oneline -20` often shows "ABC-123" patterns), then use that prefix

If it looks like a project name (no numbers, or doesn't match ticket patterns), leave it as-is.

## Step 1: Identify What We're Reviewing

Fetch the item from Linear:
- If it's a ticket ID (like `ABC-123`), fetch the ticket
- If it's a project name or ID, fetch the project and its tickets

**If reviewing a ticket that belongs to a project**, also fetch the full project details using get_project to understand:
- Project objectives and goals
- Overall scope and deliverables
- Whether this ticket aligns with the project's direction

---

## If Reviewing a TICKET:

### Step 1: Assess Current State

Check for these elements:

| Element | Status | Notes |
|---------|--------|-------|
| **Clear title** | ✅/❌ | Is it actionable? Does it start with a verb? |
| **Context/Background** | ✅/❌ | Do we know WHY this matters? |
| **Goal/Outcome** | ✅/❌ | Is the desired end state clear? |
| **Acceptance Criteria** | ✅/❌ | How do we know when it's done? |
| **Technical pointers** | ✅/⚠️/❌ | Any hints on where to look or how to approach? |
| **Scope clarity** | ✅/❌ | Is it clear what's NOT included? |
| **Right size** | ✅/❌ | Can this be done in a focused session, or should it be broken up? |

### Step 2: Fix the Title (Always)

**Bad titles are tickets you'll skip later.** If the title is vague, generic, or doesn't describe the actual work, fix it immediately:

Bad titles:
- "Fix bug" → "Fix: Cart total doesn't update when removing items"
- "Auth stuff" → "Add OAuth2 login with Google provider"
- "Performance" → "Reduce dashboard load time by lazy-loading charts"
- "Update UI" → "Replace date picker with native input on mobile"

Good titles:
- Start with an action verb (Add, Fix, Remove, Update, Refactor, Implement)
- Describe the specific outcome, not the category
- Are scannable in a list without reading the description

**Always update the title in Linear if it needs improvement** - don't just propose it, change it.

### Step 3: Fill in the Gaps (Interactive)

If the ticket is missing context, **don't just report it - help fix it**:

1. **Explore the codebase** to find relevant technical context (for documentation purposes only - do not make changes):
   - Search for related files, components, or code
   - Understand the current implementation
   - Identify dependencies or related systems

2. **Gather details using AskUserQuestion with selectable options** wherever possible:
   - Scope choices (which components/features affected)
   - UI/UX approach options
   - Behavior choices (live vs batch, sync vs async, etc.)
   - Only use open-ended questions for things that truly need free-form answers (e.g., "What problem does this solve?")

3. **Draft improvements** based on what you learn:
   - Better title (action verb + clear description)
   - Context section (why this matters)
   - Goal (what we're achieving)
   - Acceptance criteria (testable "done" conditions)
   - Technical notes (files, dependencies, approach hints)

### Step 4: Present the Enhanced Ticket

**Ticket:** [ID - Title]
**Project:** [Parent project if any]
**Current Readiness:** 🟢 Ready / 🟡 Needs Work / 🔴 Not Actionable

**What exists:**
[Summary of what the ticket currently says]

**What I found/learned:**
- [Relevant code or context discovered]
- [Technical details from codebase exploration]

**Proposed Update:**

**Title:** [Improved title if needed]

**Description:**
```markdown
## Context
[Why this matters / background - filled in from conversation + codebase]

## Goal
[What we're trying to achieve]

## Acceptance Criteria
- [ ] [Specific, testable criteria]
- [ ] [Another criterion]

## Technical Notes
[Files involved, dependencies, implementation hints from codebase exploration]
```

**Use AskUserQuestion to get decisions and confirm understanding.**

The AskUserQuestion tool supports 1-4 questions. Structure it like this:

1. **First, ask any decision questions** you surfaced during exploration. Each should have concrete, selectable options. For example:
   - "Which scope should this cover?" → Options: "Just X", "X and Y", "Full system"
   - "What's the priority behavior?" → Options: "Option A", "Option B"
   - "Should this include...?" → Options: "Yes, include it", "No, keep it simple"

2. **Last question: confirm the proposed update** - Add this as your final question:
   - Question: "Does this capture the ticket correctly?"
   - Options: "Yes, update the ticket", "Needs changes"

**Important**: Do NOT put decision questions in plain text and then ask a generic yes/no. The specific decisions MUST be selectable options in AskUserQuestion so the user can answer them directly.

If you have no open questions, just ask the single confirmation question.

Only update the ticket in Linear after user confirms via AskUserQuestion.

---

## If Reviewing a PROJECT:

### Fetch Project Data

1. First, fetch the project by name to get its full details and **project ID**
2. Then, fetch ALL tickets for the team and filter by that project ID
   - Don't rely on project name matching - use the ID from step 1
   - This ensures you get all tickets even with partial name matches

### Assess Project Health

**Project Structure:**
- Does the project have clear objectives?
- Are deliverables well-defined?
- Are success signals measurable?

**Ticket Audit:**

For each ticket, quickly assess:

| Ticket | Status | In Scope? | Well-Defined? | Issues |
|--------|--------|-----------|---------------|--------|
| ABC-1 | Todo | ✅/❌ | ✅/🟡/❌ | [notes] |
| ABC-2 | In Progress | ✅/❌ | ✅/🟡/❌ | [notes] |

**Scope Creep Check:**
- Do all tickets align with the project's stated objectives?
- Are there tickets that belong in a different project?
- Are there tickets that are actually separate initiatives?

**Completeness Check:**
- Are there obvious gaps in the tickets needed to achieve the objectives?
- Are dependencies between tickets clear?

### Present Findings

**Project:** [Name]
**Health:** 🟢 Solid / 🟡 Needs Attention / 🔴 Needs Restructuring

**Summary:**
[2-3 sentence overview of project state]

**Out of Scope Tickets:** (should be moved or removed)
- [Ticket] - [Why it doesn't fit]

**Half-Baked Tickets:** (need fleshing out)
- [Ticket] - [What's missing]

**Missing Tickets:** (gaps in coverage)
- [Suggested ticket] - [What it would cover]

**Recommendations:**
1. [Priority action 1]
2. [Priority action 2]
3. [Priority action 3]

**Use AskUserQuestion (multiSelect: true):** "What should I help with?"
- Move out-of-scope tickets to the right place
- Flesh out half-baked tickets (I'll go through each interactively)
- Create the missing tickets

---

## Fleshing Out Half-Baked Tickets (if selected)

For each half-baked ticket, **follow the create-ticket workflow**:

1. **Show the ticket** and what's missing
2. **Explore the codebase** for relevant context (read-only - do not make changes)
3. **Gather details using AskUserQuestion with selectable options** wherever possible:
   - Scope choices (which components/features affected)
   - UI/UX approach options
   - Behavior choices (live vs batch, sync vs async, etc.)
   - Only use open-ended questions for things that truly need free-form answers
4. **Draft an enhanced version** with proper structure (Context, Goal, Acceptance Criteria, Technical Notes)
5. **Confirm and update** before moving to the next ticket

Work through them one at a time. Don't batch-update without conversation.

---

## Step 5: Offer to Implement (Single Ticket Only)

**Only do this step if you audited a single ticket (not a project).**

After the ticket has been updated in Linear, use AskUserQuestion to ask:

**Question:** "The ticket is ready. Want to implement it now?"
**Options:**
- "Yes, let's implement" - Proceed to implementation using the research you've already gathered
- "No, just the audit" - End here

### If "Yes, let's implement":

1. **Mark the ticket "In Progress"** in Linear (if not already)

2. **Skip redundant research** - You've already explored the codebase during the audit. Use that context directly.

3. **Present your implementation approach** based on what you learned:
   - Current state (from your audit exploration)
   - The changes needed
   - Relevant files you already identified
   - Your suggested approach

4. **Use AskUserQuestion to confirm** before starting:
   - Include any implementation decisions as selectable options
   - Final question: "Ready to proceed with this approach?"

5. **Implement the changes** after confirmation

6. **After implementation - IMPORTANT:**
   - **Leave the ticket as "In Progress"** - Do NOT mark it Done or Completed
   - **Do NOT create git commits** - Let the user review and test first
   - Present a summary of what you changed
   - Ask the user to review and test

The user will decide when to commit and close the ticket after they've verified the changes work.
