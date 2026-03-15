You are closing out work on the current Linear ticket.

**This command writes a comment, moves to Done, commits, and pushes. No confirmation needed — just do it.**

**Default behavior:** comment → mark Done → commit → push. If the user wants different behavior (e.g., mark as "In Review", skip push), they'll say so in $ARGUMENTS.

## Step 1: Identify the Ticket

Determine which ticket we've been working on by:
- Checking the current git branch name (often contains ticket ID like `ABC-123`)
- Looking at recent conversation context
- If unclear, ask the user which ticket to close

Fetch the ticket from Linear to get the full context.

## Step 2: Review What Was Done

Look at the actual changes made during this session:
- Run `git status` to see modified/added files
- Run `git diff --stat` to understand the scope of changes
- Review the conversation to capture what was accomplished

Build a clear picture of what was implemented, fixed, or changed.

## Step 3: Generate Comment

Create a detailed comment for the Linear ticket:

```markdown
[One plain-language sentence explaining what this ticket accomplishes for a non-technical reader]

## Completed

[2-4 bullet points of what was done]

## Changes

- `path/to/file.ts` - [what changed]
- `path/to/other.ts` - [what changed]

## Decisions Made

[If any decisions were made during implementation, document them]
- **[Decision]**: [rationale - what pattern it matches or why it's simpler]

## Testing

[How this was tested or verified]
```

## Step 4: Execute (No Confirmation Needed)

Do all of this without asking:

1. **Post the comment** to Linear
2. **Update ticket status** to "Done" (unless user specified a different status in arguments)
3. **Commit changes** — stage all, generate commit message, commit
4. **Push** to remote
5. **Report back** with a brief summary: ticket link, commit hash, push status

## Step 5: What's Next?

After closing the ticket:

1. **Find the project** this ticket belongs to (you already fetched the ticket in Step 1)
2. **List remaining tickets** in the project that are not Done/Cancelled — show them in a table (ticket ID, title, status)
3. **If there are no more tickets**: Tell the user there are no remaining tickets in the project — it's all done!

## Branch Management

**Do NOT create or switch git branches.** Commit and push on whatever branch the user is currently on. The user manages their own branches.

## Edge Cases

- **No changes?** Still close the ticket if the work is done (e.g., investigation tickets), skip commit offer
- **Ticket already Done?** Just add the comment, don't change status
- **Nothing to commit?** Skip the commit offer
- **Ticket has no project?** Skip the "what's next" step
