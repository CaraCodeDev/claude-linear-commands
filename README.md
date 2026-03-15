# Claude Code Linear Commands

Custom slash commands for managing Linear tickets and projects with Claude Code.

## Prerequisites

- [Claude Code](https://claude.com/claude-code) CLI installed
- [Linear MCP server](https://github.com/anthropics/linear-mcp) configured

## Installation

Copy these `.md` files to your Claude Code commands directory:

```bash
# Global (available in all projects)
cp *.md ~/.claude/commands/

# Or project-specific
cp *.md your-repo/.claude/commands/
```

---

## Core Workflow

The recommended workflow is three commands: **create the work → do the work → close the work**.

### 1. Create the Work

**For a larger initiative:** `/project-kickoff <idea>`

```
/project-kickoff user dashboard with activity metrics and notifications
```

- Has a back-and-forth brainstorming conversation (not a checklist)
- Explores your codebase to inform the discussion
- Drafts a comprehensive project overview (the source of truth for all tickets)
- Creates phase tickets with a naming convention (`1. Backend: Implement X`)
- Includes testing, QA, and optionally docs tickets

**For a single task:** `/create-ticket <idea>`

```
/create-ticket add a button to export data as CSV
```

- Detects your team from the current repo
- Assesses scope (with context-aware breakdown options if too big)
- Asks clarifying questions with selectable options
- Drafts a structured ticket (Context, Goal, Acceptance Criteria)
- Creates the ticket after your approval

---

### 2. Work the Ticket

**Start work:** `/start-ticket <id>`

```
/start-ticket ABC-123
/start-ticket 123        # infers team prefix from git history
/start-ticket abc123     # handles missing hyphen
```

- Fetches the ticket, comments, parent ticket context, and project overview
- Marks it "In Progress"
- Enters plan mode and explores your codebase
- Writes a detailed implementation plan
- Resolves open questions with selectable options
- Waits for your approval before implementing
- After implementation, checks if the project overview needs updating

---

### 3. Close the Ticket

**Wrap up:** `/close-ticket`

```
/close-ticket
/close-ticket in review   # override default Done status
```

- Identifies the ticket from your branch or conversation
- Reviews `git diff --stat` to see what changed
- Generates a detailed Linear comment (summary, changes, decisions, testing)
- Posts comment → marks Done → commits → pushes (no confirmation needed)
- Shows remaining tickets in the project

---

## Other Useful Commands

| Command | Purpose |
|---------|---------|
| `/create-doc <topic>` | Research the codebase and write user-facing documentation (how-to guides, feature overviews, getting started pages) with screenshot placeholders |
| `/simplify-ticket <id>` | Find all files changed for a completed ticket or project (via git log, comments, branches) and present them for review and simplification |
| `/commit` | Standalone commit — auto-detects ticket context from branch/conversation, stages, commits, and pushes with a formatted message |
| `/audit <id>` | Review a ticket or project for quality — checks titles, scope, acceptance criteria, and offers to fix gaps interactively |
| `/triage` | Process the triage queue — assess completeness, fix titles, route to projects, identify Quick Wins |

---

## Archive

The `archive/` directory contains experimental commands no longer part of the main workflow:

- **plan-phase** - Just-in-time planning for project phases
- **run-phase** - Autonomous execution of a phase of tickets
- **commit-phase** - Single commit for all work in a phase
- **start-ticket-no-plan** - Start ticket without plan mode

---

## Tips

- Commands auto-detect your team from the git remote
- Ticket IDs are flexible: `ABC-123`, `abc-123`, `abc123`, or just `123`
- Most questions use selectable options — faster than typing
- The commands explore your codebase to add relevant technical context
- `/close-ticket` and `/commit` execute without confirmation by default
- Project overviews are the source of truth — tickets reference them, not the other way around
