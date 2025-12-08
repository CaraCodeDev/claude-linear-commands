You are closing out work on the current Linear ticket.

## Step 1: Identify the Ticket

Determine which ticket we've been working on by:
- Checking the current git branch name (often contains ticket ID like `ABC-123`)
- Looking at recent conversation context
- If unclear, ask the user which ticket to close

Fetch the ticket from Linear to get the full context.

## Step 2: Review What Was Done

Look at the actual changes made during this session:
- Run `git status` to see modified/added files
- Run `git diff` to understand the changes
- Review the conversation to capture what was accomplished

Build a clear picture of what was implemented, fixed, or changed.

## Step 3: Generate Summary

Create a concise summary of the work:

**For the Linear comment:**
```markdown
## Completed

[2-4 bullet points of what was done]

## Changes

- `path/to/file.ts` - [what changed]
- `path/to/other.ts` - [what changed]

## Testing

[How this was tested or verified, if applicable]
```

**For the commit message:**
```
<ticket-id>: <type>(scope) - <short description>

<ticket-id>: <what was accomplished>

- Key change 1
- Key change 2

- Link to the ticket

🤖 Generated with Claude Code
```

Where type is: `Fix`, `Feature`, `Refactor`, `Docs`, `Chore`, `UX/UI`, `Bug`, etc.

## Step 4: Confirm with User

Present:
- **Ticket:** [ID and title]
- **Linear Comment:** [the formatted comment]
- **Commit Message:** [the formatted commit]
- **Files to commit:** [list from git status]

**Ask about review status using AskUserQuestion tool:**
1. **Changes?**
2. **Close it** - Mark as Done (no review needed)
2. **Send to review** - Move to In Review status
3. **Send to review with comments** - Move to In Review and ask what comment to leave for reviewers

## Step 5: Execute (after confirmation)

1. **Post comment** to Linear with the summary
2. **Update ticket status** based on user's choice:
   - If closing: Set to "Done"
   - If review: Set to "In Review"
   - If review with comments: Set to "In Review", then post a second comment with the user's note for reviewers
3. **Stage all changes:** `git add .`
4. **Commit** with the formatted message including ticket ID
5. **Report back** with:
   - Link to the ticket
   - Commit hash
   - Current ticket status
   - Ask if user wants to push to remote

## Edge Cases

- **No changes to commit?** Skip git steps, just comment and close the ticket
- **Uncommitted changes from other work?** Include it too
- **Branch doesn't match ticket?** Confirm with user before proceeding
