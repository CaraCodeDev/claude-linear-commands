You are going back to simplify code from a previously completed Linear ticket or project: $ARGUMENTS

## Step 0: Normalize Input

If the input looks like a ticket ID (contains numbers), normalize it:

1. **Uppercase it** - "abc-11" → "ABC-11"
2. **Add hyphen if missing** - "abc11" → "ABC-11"
3. **Infer team prefix if just a number** - check `git log --oneline -20` for recent prefixes

If it looks like a project name, leave as-is.

## Step 1: Load Context from Linear

**If it's a ticket:**
- Fetch the ticket with `get_issue` and `list_comments` in parallel
- Extract what was done from the ticket description and comments (close-ticket comments list specific files and changes)

**If it's a project:**
- Fetch the project and all its tickets
- Gather comments from all Done/completed tickets
- Build a combined picture of all files that were touched

## Step 2: Find Changed Files

Use multiple strategies to identify the files that were changed, in order of reliability:

1. **Git log** - Search for commits mentioning the ticket ID:
   ```
   git log --all --oneline --grep="TICKET-ID"
   ```
   Then get the files from those commits:
   ```
   git log --all --name-only --pretty=format: --grep="TICKET-ID"
   ```

2. **Ticket comments** - The `/close-ticket` command writes a "Changes" section listing files. Parse these from comments.

3. **Branch inspection** - Look for branches named after the ticket:
   ```
   git branch -a | grep -i "ticket-id"
   ```

Deduplicate and filter to files that still exist. If a file was deleted or renamed, skip it.

**For projects:** Aggregate files across all tickets. This may be a large set — that's fine.

## Step 3: Assess Scope

Present what you found:

> **Simplify: [Ticket/Project Title]**
>
> Found **N files** changed across **M commits**:
> - `path/to/file1.ts`
> - `path/to/file2.ts`
> - ...

If there are more than 15 files, group them by directory and note the count. For projects with many files, suggest doing it in batches by ticket.

**Stop here.** Do not make any code changes. The user will decide what to do next. Prompt the user to run /simplify 

## Edge Cases

- **File no longer exists?** Skip it from the list silently
- **Can't find any commits for the ticket?** Fall back to ticket comments for file lists. If still nothing, ask the user which files to look at
- **Ticket is still In Progress?** Warn the user but proceed if they confirm
