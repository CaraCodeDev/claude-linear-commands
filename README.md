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
- Assess if it's ticket-sized work
- Ask clarifying questions with selectable options
- Draft a structured ticket (Context, Goal, Acceptance Criteria)
- Suggest appropriate labels
- Create the ticket after your approval

**For a larger initiative:** `/project-kickoff <idea>`

```
/project-kickoff user dashboard with activity metrics and notifications
```

The command will:
- Walk through objectives, deliverables, and success signals
- Draft a project with structured documentation
- Suggest 3-6 initial tickets to get started
- Create the project and optionally the starter tickets

---

### 2. Work the Ticket

**Start work:** `/start-ticket <id>`

```
/start-ticket ABC-123
/start-ticket 123        # infers team prefix from git history
/start-ticket abc123     # handles missing hyphen
```

The command will:
- Fetch the ticket and mark it "In Progress"
- Explore your codebase to understand the current state
- Present its understanding: relevant files, the problem, suggested approach
- Ask clarifying questions if there are ambiguities
- Wait for your confirmation before implementing

After implementation, it leaves the ticket in progress and doesn't commit—you stay in control of the review.

---

### 3. Close the Ticket

**Wrap up:** `/close-ticket`

```
/close-ticket
```

The command will:
- Identify the ticket from your branch or conversation
- Review `git diff` to see what changed
- Generate a Linear comment summarizing the work
- Draft a commit message with the ticket ID
- Ask if you want to mark it Done or send to Review
- Post the comment, update status, and commit

---

## Additional Commands

### Audit: Review & Improve

**Review a ticket or project:** `/audit <id or name>`

```
/audit ABC-123           # audit a single ticket
/audit My Project        # audit a project and all its tickets
```

For tickets, it checks for clear title, context, acceptance criteria, and technical notes—then helps fill gaps by exploring your codebase and asking targeted questions.

For projects, it audits all tickets for scope alignment, identifies half-baked tickets, spots gaps, and offers to fix them interactively.

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
- Enhance vague tickets with structured details
- Route to Backlog or Todo based on your call

Useful for processing client-submitted tickets from Linear Asks.

---

## Command Reference

| Command | Purpose | When to use |
|---------|---------|-------------|
| `/create-ticket <idea>` | Create a new ticket | Starting focused work |
| `/project-kickoff <idea>` | Create a project with tickets | Starting a larger initiative |
| `/start-ticket <id>` | Begin work on a ticket | Ready to implement |
| `/close-ticket` | Summarize, commit, close | Done with implementation |
| `/audit <id>` | Review and improve | Ticket/project needs refinement |
| `/triage` | Process incoming queue | New tickets need routing |

---

## Tips

- Commands auto-detect your team from the git remote
- Ticket IDs are flexible: `TAC-123`, `tac-123`, `tac123`, or just `123`
- Most questions use selectable options—faster than typing
- The commands explore your codebase to add relevant technical context
- Nothing is created or updated without your confirmation
