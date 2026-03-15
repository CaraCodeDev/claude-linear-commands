You are helping create user-facing documentation. The topic: $ARGUMENTS

## Step 1: Research the Codebase

Explore the codebase to understand the feature/workflow the user described. Look for:
- Routes, pages, and components related to the topic
- API endpoints and data flows
- User-facing UI elements, forms, buttons, modals
- Permissions, roles, or prerequisites
- Edge cases, limits, or important constraints

Be thorough — the doc quality depends on understanding the actual implementation.

## Step 2: Determine Doc Type

Based on what you found, pick the structure that fits best:

**How-to guide** (task-oriented, the user wants to DO something):
```
# [Task Title]

## Overview
One-two sentences on what this lets you do.

## Before You Start
Prerequisites, permissions, or setup needed.

## Steps
1. Step one...
2. Step two...
   <!-- screenshot: description of what to capture -->
3. Step three...

## Good to Know
Tips, edge cases, or common questions.
```

**Feature overview** (conceptual, explaining what something does/how it works):
```
# [Feature Name]

## Overview
What this feature is and what it does for you.

## How It Works
Explain the workflow/process in plain language.
<!-- screenshot: description of what to capture -->

## Key Details
Important things to know — limits, behaviors, settings.

## Good to Know
Tips, edge cases, or common questions.
```

**Getting started** (onboarding, sequential setup):
```
# [Getting Started Title]

## Overview
What you'll set up and what you'll be able to do after.

## Step 1: [First Thing]
...
<!-- screenshot: description of what to capture -->

## Step 2: [Next Thing]
...

## Step 3: [Next Thing]
...

## What's Next
Where to go from here, links to related docs.
```

These are starting points, not rigid templates. Adapt sections as needed — skip what doesn't apply, add what does.

## Step 3: Propose the File Path

Based on the topic, propose a category folder and filename:

```
I'll save this to /docs/<category>/<slug>.md — sound good?
```

- **Category**: Pick a short, descriptive folder name based on the product area (e.g., `posting`, `admin`, `account`, `reviews`). Check `/docs/` for existing folders first and reuse them when the topic fits.
- **Filename**: Kebab-case, descriptive but not too long.

**Wait for the user to confirm or redirect before writing anything.**

## Step 4: Write the Doc

### Tone & Style
- Aim for Notion-level docs: friendly, clear, scannable. Not corporate, not overly casual.
- Write directly to the user ("you can", "your account") — not third person.
- Short paragraphs. Use bullets and numbered steps liberally.
- Don't pad. Say what needs to be said, then stop.
- Bold key terms or UI elements on first mention (e.g., **Post Review**, **Export**).

### Screenshots
- Place `<!-- screenshot: description -->` comments where a visual would help.
- Don't reference screenshots in the body text — they're supplementary, not load-bearing. The doc should make sense without them.

### Content
- Base everything on what you found in the codebase. Don't invent features or behaviors.
- If something is unclear from the code, note it and ask the user rather than guessing.
- Focus on what the user needs to know, not implementation details.

## Step 5: Present the Doc

After writing the file, give a brief summary of what you documented and flag anything you weren't sure about or couldn't determine from the code.
