You are committing a phase of work. The planning ticket: $ARGUMENTS

**This command creates a single commit for all work in a phase, using ticket comments as the source.**

## Step 0: Normalize Input

The input is a **planning ticket ID** - the B-1/E-1/etc. ticket for the phase.

Example: `/commit-phase ABC-25` where ABC-25 is "Feature Module: E-1 Plan estimation flow..."

Normalize the ticket ID:
1. **Uppercase it** - "abc-25" → "ABC-25"
2. **Add hyphen if missing** - "abc25" → "ABC-25"
3. **Infer team prefix if just a number** - check git history for recent ticket prefixes

## Step 1: Load Phase Context

1. **Fetch the planning ticket** to extract:
   - Phase name (e.g., "Feature Module")
   - Phase letter (e.g., "E")
   - Project it belongs to

2. **Find the implementation tickets**:
   - Use `list_issues` with:
     - `project`: the project ID
     - `state`: "Done" (we're committing completed work)
     - `query`: the phase prefix (e.g., "Feature Module: E-")
     - `limit`: 20
   - Filter to ticket number > 1 (exclude planning ticket)
   - Order by ticket number

3. **For each implementation ticket**, fetch its comments using `list_comments`:
   - Look for the completion comment (has "## Completed" section)
   - Extract: summary bullets, files changed, decisions made

## Step 2: Check Git Status

Run `git status` to verify:
- There are uncommitted changes
- The changes align with the tickets being committed

If no changes, inform user and exit.

## Step 3: Build Commit Message

Structure:
```
[Phase Name]: [High-level description of what the phase delivered]

Tickets completed:
- [ABC-50] E-5 [Title]
- [ABC-51] E-6 [Title]
- [ABC-52] E-7 [Title]
...

Key features:
- [Synthesize from ticket comments - what was built]
- [Major functionality delivered]
- [Important patterns or approaches used]

Key decisions:
- [Decision]: [Rationale] (from ticket E-X)
- [Decision]: [Rationale] (from ticket E-Y)

🤖 Generated with Claude Code

Co-Authored-By: Claude <assistant@anthropic.com>
```

**Guidelines for the commit message:**
- Title should describe the deliverable, not list tickets
- Key features should be user-facing or architecturally significant
- Key decisions should include 3-5 most important decisions from across tickets
- Keep it scannable - someone should understand the phase in 30 seconds

## Step 4: Present and Confirm

Show the user:
- **Phase:** [Name]
- **Tickets included:** [count] tickets
- **Files to commit:** [from git status]
- **Commit message:** [the formatted message]

**Ask:** "Create this commit?"

## Step 5: Execute

After confirmation:

1. **Stage all changes:** `git add .`
2. **Create the commit** with the formatted message (use HEREDOC for multi-line)
3. **Report back:**
   - Commit hash
   - Files committed
   - Ask if user wants to push

## Edge Cases

- **Some tickets not Done?** List them and ask if user wants to proceed with partial commit
- **No completion comments on tickets?** Use ticket titles and git diff to infer what was done
- **Mixed phases in uncommitted changes?** Warn user, suggest committing one phase at a time
- **Already committed?** Check if HEAD commit matches this phase, inform user if so
