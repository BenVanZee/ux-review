---
name: ux-review:build-workflows
description: Guided interview to document a business workflow, with optional browser validation. Produces structured workflow docs that make future UX reviews workflow-aware.
---

# Build Workflows

Document a business workflow through guided conversation, then optionally validate it by walking the live app in a browser.

## Usage

```
/ux-review:build-workflows                    # Asks which workflow to document
/ux-review:build-workflows morning-funding    # Names the workflow upfront
```

## Process

### 1. Check Existing Workflows

Read `docs/workflows/` in the project root. If workflows already exist, show a quick summary so the user knows what's already documented and can avoid duplicates.

### 2. Interview

Ask questions **one at a time**. Wait for each answer before asking the next.

1. **What is this workflow?** — Ask for a name and a one-sentence description of what it accomplishes.
2. **Who performs it?** — Role or specific person (CSA, account manager, etc.)
3. **When/what triggers it?** — "Every morning", "When a client submits a schedule", "When a new carrier calls", etc.
4. **Walk me through the steps.** — Let the user describe the flow naturally. Ask follow-up questions to:
   - Clarify ambiguous steps ("When you say 'check the schedule,' what screen are you on?")
   - Identify which screens/pages are involved
   - Fill gaps between steps ("What happens between verifying and funding?")
   - Understand decision points ("What determines whether you approve or reject?")
5. **What does success look like?** — How do you know this workflow completed correctly?
6. **What goes wrong?** — Common failure modes, edge cases, frustrations.
7. **What matters most?** — Speed? Accuracy? Auditability? Where does the pain concentrate?

### 3. Write the Workflow Doc

Create `docs/workflows/<workflow-name>.md` (kebab-case filename). Use this format:

```markdown
# <Workflow Name>

**Performed by:** <role or person>
**Trigger:** <what starts this workflow>
**Frequency:** <how often — daily, on-demand, quarterly, etc.>
**App:** <which application>
**Success:** <what a successful completion looks like>

## Steps

1. **<Step name>** — <description>
   - Screen: `<route or page>`
   - What matters: <what the user cares about at this step>

2. **<Step name>** — <description>
   - Screen: `<route or page>`
   - What matters: <what the user cares about at this step>

## Pain Points
- <known frustrations, workarounds, bottlenecks>

## What Matters Most
<1-2 sentences on the priority — speed, accuracy, auditability, etc.>

## Validation Notes
<!-- Added by browser walkthrough, if performed -->
```

Create the `docs/workflows/` directory if it doesn't exist.

Show the user a summary of what was documented.

### 4. Optional Browser Validation

After saving the doc, ask:

> "Want me to walk this flow in the browser to see if the UI matches what you described? I'll need the app URL."

If the user accepts:

1. Open the app via Playwright (preferred) or Claude in Chrome
2. Follow the documented steps sequentially
3. Take a screenshot at each step
4. For each step, check:
   - Can the step be performed as described?
   - How many clicks/actions does it take?
   - Is the navigation path obvious or buried?
   - Are there extra steps the workflow doc didn't mention?
5. Append findings to the **Validation Notes** section of the workflow doc
6. Flag any mismatches between the documented workflow and the actual UI

If the user declines, the workflow doc is complete as-is. The validation is a bonus, not a requirement.

## Important

- **One workflow per session.** Keep it focused. If the user mentions adjacent workflows, note them and suggest documenting them separately.
- **Don't assume screen paths.** If the user doesn't know the exact route, leave the Screen field as `<unknown>` — it can be filled in during browser validation or later.
- **Use the user's language.** Match their terminology in the doc, not generic UX terms.
- **Create the directory** `docs/workflows/` if it doesn't exist.
