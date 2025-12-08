You are helping create a well-structured Linear Project. The user's idea: $ARGUMENTS

## Step 1: Detect Context

1. **Fetch available teams** from Linear using the list_teams tool
2. **Check the current working directory** and git remote to infer which team this belongs to
3. If you can confidently match the repo/folder to a team, use that team
4. If unclear, **use AskUserQuestion** to let the user pick from the available teams

## Step 2: Understand the Initiative

Have a conversation to understand the initiative. **Use AskUserQuestion with selectable options where possible:**

1. **What's the big picture?** What capability or outcome are we building toward? (open-ended)
2. **Who benefits?** Use AskUserQuestion (multiSelect: true): Users / Operators / Admins / Developers
3. **What are the key deliverables?** What are the major pieces of work? (open-ended, or offer patterns you see in the codebase)
4. **Any dependencies?** Does this build on existing features or require other work first?
5. **How do we know it's done?** What are the success signals?

Mix selectable options with open-ended questions - don't ask everything at once.

## Step 3: Draft the Project

Based on the conversation, structure the project:

**Name:** Clear, descriptive name for the initiative

**Summary:** One-line description (max 255 chars) - what this project delivers

**Description:** Structured as:
```markdown
## Objectives

[2-4 bullet points on what we're trying to achieve]

## Core Deliverables

### 1. [First Major Deliverable]
- Key features/tasks
- Specific functionality

### 2. [Second Major Deliverable]
- Key features/tasks
- Specific functionality

[Continue as needed]

## Technical Notes

[Architecture considerations, dependencies, constraints]

## Success Signals

- [Measurable outcome 1]
- [Measurable outcome 2]

## Dependencies

- [Other projects, features, or external factors this depends on]
```

## Step 4: Identify Initial Tickets

Based on the deliverables, suggest 3-6 initial tickets to get started:
- These should be concrete, actionable first steps
- Mix of foundation work and early wins
- Don't try to plan everything - just enough to start

Format each as:
- **Title:** [Action-oriented title]
- **Purpose:** [One line on what this accomplishes]

## Step 5: Confirm and Create

Show the user:
- Team: [detected team]
- Project Name: [name]
- Summary: [summary]
- Description: [full description]
- Initial Tickets: [list of suggested tickets]

Ask: "Does this look good? Any changes before I create the project?"

After confirmation:
1. Create the project in Linear
2. Use AskUserQuestion: "Create the initial tickets now?" → Yes, create them / No, just the project
