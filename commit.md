You are creating a git commit. Optional ticket IDs: $ARGUMENTS

**This is a flexible commit command that automatically picks up context. No confirmation needed — just stage, commit, and push.**

## Step 1: Gather Context

Look for ticket context from multiple sources (in priority order):

1. **Explicit arguments** - If user provided ticket IDs, use those
2. **Conversation context** - Look for tickets worked on this session:
   - Recent `/start-ticket` or `/close-ticket` invocations
   - Ticket IDs mentioned in conversation
   - Linear API calls made during session
3. **Git branch name** - Often contains ticket ID (e.g., `abc-55-add-feature`)
4. **Recent commits** - Check `git log -5 --oneline` for ticket patterns

Normalize any ticket IDs found:
- Uppercase: "abc-55" → "ABC-55"
- Add hyphen if missing: "abc55" → "ABC-55"

## Step 2: Fetch Ticket Details (if found)

If ticket IDs were identified:
- Fetch each ticket from Linear
- Get title, description
- Check for recent completion comments (from `/close-ticket`)
- Extract: what was done, decisions made

## Step 3: Review Changes

Run in parallel:
- `git status` - see what's staged/unstaged
- `git diff --stat` - scope of changes
- `git diff` (if small) or `git diff --name-only` (if large) - understand what changed

Build understanding of what the commit contains.

## Step 4: Build Commit Message

**If ticket found (ticket-first format):**
```
ABC-55: feat(module) - add room selection flow

[What was accomplished - 1-2 sentences]

- Key change 1
- Key change 2
- Key change 3

🤖 Generated with Claude Code

Co-Authored-By: Claude <assistant@anthropic.com>
```

**If multiple tickets:**
```
ABC-55, ABC-56: feat(module) - add room components

[What was accomplished - 1-2 sentences]

- Key change 1
- Key change 2

🤖 Generated with Claude Code

Co-Authored-By: Claude <assistant@anthropic.com>
```

**If no tickets (conventional commits format):**
```
feat(module): add room selection flow

[What was accomplished - 1-2 sentences]

- Key change 1
- Key change 2

🤖 Generated with Claude Code

Co-Authored-By: Claude <assistant@anthropic.com>
```

**Types:** `feat`, `fix`, `refactor`, `docs`, `chore`, `style`, `test`

**Title format rules:**
- With ticket: `TICKET-ID: type(scope) - description`
- Without ticket: `type(scope): description`
- Keep under 72 characters
- Use imperative mood ("add" not "added")

**Guidelines:**
- Title should be concise (<72 chars)
- Type should match the nature of changes
- Bullets should highlight significant changes, not list every file
- If decisions were made, include the important ones

## Step 5: Execute (No Confirmation Needed)

Do all of this without asking:

1. **Stage changes:** `git add .`
2. **Create commit** using HEREDOC for message
3. **Push** to remote
4. **Report back** briefly: commit hash, summary, push status

## Smart Behaviors

- **Multiple tickets?** Group them logically in the message
- **Ticket already has completion comment?** Pull summary from there
- **No tickets but branch has ticket ID?** Use that for context
- **Large diff?** Focus on architectural changes, not file-by-file
- **Only docs/config changed?** Use appropriate type (docs/chore)

## Edge Cases

- **Nothing to commit?** Inform user, exit gracefully
- **Unstaged changes only?** Ask if user wants to stage all or select files
- **Ticket not in Done status?** Note it but proceed (user might be committing WIP)
- **Can't determine ticket?** That's fine, create ad-hoc commit
