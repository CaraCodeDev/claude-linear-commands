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

## Workflow Lifecycle

### 1. Create Work

**For a single task:** `/create-ticket <idea>`

```
/create-ticket add a button to export data as CSV
```

The command will:
- Detect your team from the current repo
- Assess if it's ticket-sized work (with context-aware breakdown options if too big)
- Ask clarifying questions with selectable options
- Draft a structured ticket (Context, Goal, Acceptance Criteria)
- Suggest appropriate labels
- Create the ticket after your approval

**For a larger initiative:** `/project-kickoff <idea>`

```
/project-kickoff user dashboard with activity metrics and notifications
```

The command will:
- Have a back-and-forth brainstorming conversation (not a checklist)
- Explore your codebase to inform the discussion
- Draft a comprehensive project overview (the source of truth for all tickets)
- Create phase tickets with a naming convention (`1. Backend: Implement X`)
- Include testing, QA, and optionally docs tickets

---

### 2. Work the Ticket

**Start work:** `/start-ticket <id>`

```
/start-ticket ABC-123
/start-ticket 123        # infers team prefix from git history
/start-ticket abc123     # handles missing hyphen
```

The command will:
- Fetch the ticket, comments, parent ticket context, and project overview
- Mark it "In Progress"
- Enter plan mode and explore your codebase
- Write a detailed implementation plan
- Resolve open questions with selectable options
- Wait for your approval before implementing
- After implementation, check if the project overview needs updating

---

### 3. Close the Ticket

**Wrap up:** `/close-ticket`

```
/close-ticket
/close-ticket in review   # override default Done status
```

The command will:
- Identify the ticket from your branch or conversation
- Review `git diff --stat` to see what changed
- Generate a detailed Linear comment (summary, changes, decisions, testing)
- Post comment → mark Done → commit → push (no confirmation needed)
- Show remaining tickets in the project

---

### 4. Commit (standalone)

**Quick commit:** `/commit`

```
/commit
/commit ABC-123          # explicit ticket ID
```

The command will:
- Auto-detect ticket context from branch, conversation, or recent commits
- Fetch ticket details for the commit message
- Stage, commit, and push (no confirmation needed)
- Format: `ABC-123: feat(scope) - description`

---

## Additional Commands

### Audit: Review & Improve

**Review a ticket or project:** `/audit <id or name>`

```
/audit ABC-123           # audit a single ticket
/audit My Project        # audit a project and all its tickets
```

For tickets: checks title, context, acceptance criteria, scope, and size. If the ticket is too big, offers to break it into phases or a full project. After auditing, offers to implement immediately.

For projects: audits all tickets for scope alignment, phase ordering, naming conventions, and project overview accuracy. Offers to fix issues interactively.

---

### Triage: Process Incoming Tickets

**Work through the triage queue:** `/triage`

```
/triage
```

The command will:
- Let you select a team
- Show all tickets in "Triage" status
- For each ticket: assess completeness, suggest a project, identify Quick Wins
- Fix vague titles, enhance descriptions
- Route to Backlog or Todo based on your call

---

### Create Documentation

**Write user-facing docs:** `/create-doc <topic>`

```
/create-doc how to manage team permissions
```

The command will:
- Research the codebase to understand the feature
- Pick the right doc structure (how-to, feature overview, or getting started)
- Propose a file path and wait for confirmation
- Write the doc with screenshot placeholders

---

### Simplify: Review Past Work

**Revisit completed work:** `/simplify-ticket <id>`

```
/simplify-ticket ABC-123
/simplify-ticket My Project    # review all files from a project
```

Finds all files changed for the ticket/project (via git log, comments, and branches) and presents them for review. Does not make changes — you decide what to do next.

---

## Archive

The `archive/` directory contains experimental commands that are no longer part of the main workflow:

- **plan-phase** - Just-in-time planning for project phases (creates implementation tickets)
- **run-phase** - Autonomous execution of a phase of tickets (autopilot mode)
- **commit-phase** - Single commit for all work in a phase
- **start-ticket-no-plan** - Start ticket without plan mode (uses AskUserQuestion instead)

---

## Command Reference

| Command | Purpose | When to use |
|---------|---------|-------------|
| `/create-ticket <idea>` | Create a new ticket | Starting focused work |
| `/project-kickoff <idea>` | Create a project with tickets | Starting a larger initiative |
| `/start-ticket <id>` | Begin work on a ticket | Ready to implement |
| `/close-ticket` | Summarize, commit, close | Done with implementation |
| `/commit` | Stage, commit, push | Need a quick commit with ticket context |
| `/audit <id>` | Review and improve | Ticket/project needs refinement |
| `/triage` | Process incoming queue | New tickets need routing |
| `/create-doc <topic>` | Write documentation | Feature needs user-facing docs |
| `/simplify-ticket <id>` | Review past changes | Want to revisit completed work |

---

## Tips

- Commands auto-detect your team from the git remote
- Ticket IDs are flexible: `ABC-123`, `abc-123`, `abc123`, or just `123`
- Most questions use selectable options — faster than typing
- The commands explore your codebase to add relevant technical context
- `/close-ticket` and `/commit` execute without confirmation by default
- Project overviews are the source of truth — tickets reference them, not the other way around
