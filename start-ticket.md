You are starting work on Linear ticket: $ARGUMENTS

## Step 0: Normalize Ticket ID

Before fetching, normalize the input to a valid Linear ticket ID:

1. **Uppercase it** - "abc-11" → "ABC-11"
2. **Add hyphen if missing** - "abc11" → "ABC-11" (split where letters meet numbers)
3. **Infer team prefix if just a number** - "11" → check git history for recent ticket prefixes (e.g., `git log --oneline -20` often shows "ABC-123" patterns), then use that prefix

Only proceed once you have a properly formatted ticket ID like "ABC-11".

## Step 1: Load the Ticket

Fetch the ticket from Linear and extract:
- Title and description
- Project it belongs to
- Any labels, priority, or due date
- Comments or discussion
- Linked issues or parent tickets

**If the ticket belongs to a project**, fetch the full project details using get_project to understand:
- Project objectives and goals
- Overall scope and deliverables
- Any context that helps frame this ticket's role in the bigger picture

## Step 2: Mark In Progress

Update the ticket status to "In Progress" in Linear.

## Step 3: Gather Codebase Context

Based on the ticket description, explore the codebase to understand:

1. **Where does this live?** Find the relevant files, components, or modules
2. **How does it currently work?** Read the existing implementation
3. **What's connected?** Identify related code, dependencies, or side effects
4. **Any existing patterns?** Note conventions or approaches used elsewhere that we should follow

Use the codebase exploration to build a mental model - don't just skim, actually understand the current state.

## Step 4: Present Understanding

Stop and present to the user:

### Ticket Summary
> [One-line summary of what we're trying to accomplish]

### Current State
[Explain how things work now - be specific about files and code paths]

### The Problem/Gap
[What's wrong, missing, or needs to change]

### Relevant Files
- `path/to/file.ts` - [why it's relevant]
- `path/to/other.ts` - [why it's relevant]

### Initial Approach
[Your suggested approach to solving this - 2-4 bullet points]

### Questions/Concerns
[Anything unclear, risky, or that needs discussion - list them here as context]

---

### Title Check

Now that you understand what this ticket is actually about, check if the title reflects that. If the title is vague ("Fix bug", "Auth stuff", "Update UI"), **update it in Linear** to be specific and actionable (e.g., "Fix: Cart total doesn't update when removing items").

Good titles start with an action verb and describe the specific outcome. This should be rare if the ticket was already audited.

---

**Use AskUserQuestion to get decisions on your questions/concerns AND confirm understanding.**

The AskUserQuestion tool supports 1-4 questions. Structure it like this:

1. **First, ask any decision questions** you have (from Questions/Concerns above). Each should have concrete, selectable options. For example:
   - "Which approach should we take for X?" → Options: "Approach A", "Approach B"
   - "This could affect X or Y - which is the priority?" → Options: "Prioritize X", "Prioritize Y"
   - "Where should this component live?" → Options: "In /components", "In /features/X"

2. **Last question: confirm understanding** - Add this as your final question:
   - Question: "Does my understanding look correct?" (summarize the main points in the question)
   - Options: "Yes, let's proceed", "Needs clarification"

**Important**: Do NOT put decision questions in plain text and then ask a generic yes/no. The specific decisions MUST be selectable options in AskUserQuestion so the user can answer them directly.

If you have no questions/concerns, just ask the single confirmation question.

## Step 5: Wait for Confirmation

Do NOT start implementing until the user confirms the understanding is correct.

If they select "Needs clarification" or provide corrections, update your mental model and re-confirm using AskUserQuestion again if needed.

Only proceed to implementation after explicit go-ahead via the AskUserQuestion response.

## Step 6: After Implementation

When you've finished writing the code:

1. **Leave the ticket as "In Progress"** - Do NOT mark it Done or Completed
2. **Do NOT create git commits** - Let the user review changes first
3. **Present a summary**
4. **Ask for review**

The user will decide when to commit and close the ticket.
