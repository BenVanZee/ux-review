---
name: ux-review:user-script
description: Generate usability test scripts from documented workflows. Two formats — observed (facilitator + Loom walkthrough) and self-guided (async questionnaire for email/forms).
---

# User Script

Generate standalone usability test scripts from workflows documented via `/ux-review:build-workflows`.

## Usage

```
/ux-review:user-script                          # Lists workflows, asks which one
/ux-review:user-script morning-funding           # Specific workflow
/ux-review:user-script --observed                # Facilitator + Loom script only
/ux-review:user-script --self-guided             # Async questionnaire only
/ux-review:user-script --both                    # Both formats (default)
```

## Process

### 1. Load Workflow

Read `docs/workflows/` in the project root. If a workflow name is provided as an argument, find the matching file. If not, list available workflows and ask which one(s) to generate scripts for.

If no workflow docs exist, tell the user:

> "No workflow docs found in `docs/workflows/`. Run `/ux-review:build-workflows` first to document a workflow."

### 2. Generate Test Script

Create `docs/workflows/test-scripts/<workflow-name>-test.md`.

Create the `docs/workflows/test-scripts/` directory if it doesn't exist.

The file contains one or both formats depending on the flag (default: both).

---

### Observed Format (Facilitator + Loom)

For in-person or screen-share sessions where someone watches the user work.

#### Setup Instructions

> **Before you start:**
> - Open the app to the starting screen for this workflow
> - Start Loom (or your screen recorder) with camera on
> - Plan for 15-20 minutes
>
> **Your role:** You are an observer, not a helper. Don't point things out, don't guide them to the right answer. If they get stuck, ask "What are you looking for?" — don't say "Click the button on the left."

#### Intro Script (Read Aloud)

> "Thanks for helping us improve [app name]. There are no wrong answers — we're testing the software, not you. I'm going to ask you to do a few things. Think out loud as you go — tell me what you're looking at, what you expect to happen, and what you're thinking."

#### Tasks

Generate one task per workflow step. Each task has:

- **What to say:** A natural-language prompt that describes the goal without revealing how to do it. Frame it as a scenario, not an instruction.
  - Good: "A client just submitted their schedule overnight. Show me how you'd start your day."
  - Bad: "Click the Funding Queue button on the dashboard."
- **What to watch for:** Notes for the facilitator — what the user should do if the UI is working well, red flags if they hesitate, go the wrong way, or express confusion.
- **Follow-up questions:** 1-2 probing questions to ask if something interesting happens.
  - "I noticed you paused there — what were you looking for?"
  - "You went to [X] first — is that where you expected to find it?"
  - "Was that what you expected to happen?"

#### Wrap-Up Questions

> - "What was the most frustrating part of what we just did?"
> - "Was there anything that surprised you?"
> - "If you could change one thing about this tool, what would it be?"
> - "Is there anything you do as a workaround that you think shouldn't be necessary?"

#### Facilitator Notes

Leave a blank section for the facilitator to write observations during or after the session.

```markdown
## Facilitator Notes

**Date:**
**Participant:**

**Observations:**

**Key Takeaways:**
```

---

### Self-Guided Format (Async Questionnaire)

For email or Google Form distribution. No facilitator needed.

#### Intro

> We're trying to improve [app name]. Your answers help us understand how you actually use it day-to-day. There are no wrong answers — we want to know what's really happening, not what's "supposed to" happen. Takes about 10 minutes.

#### Per-Step Questions

For each workflow step, generate a question cluster:

- "When you need to **[step description]**, how do you usually do it?"
- "Is there anything about this step that slows you down or confuses you?"
- "On a scale of 1-5, how easy is this step?" (1 = frustrating, 5 = effortless)

If the workflow doc has known pain points related to this step, add a targeted question:
- "We've heard that [pain point] can be an issue. Does that match your experience? How do you deal with it?"

#### General Closing Questions

> - "What's the one thing you wish worked differently?"
> - "Is there anything you do as a workaround that you think shouldn't be necessary?"
> - "What part of [app name] do you dread using?"
> - "Is there anything about [app name] that you really like and don't want changed?"

---

## Important

- **Don't lead the witness.** Observed-mode prompts should describe goals, not steps. Self-guided questions should be open-ended, not yes/no.
- **Use the workflow's terminology.** If the workflow doc says "schedule" not "invoice batch," the test script should say "schedule."
- **Reference known pain points.** The workflow doc's Pain Points section should inform targeted questions — but phrase them as neutral probes, not assumptions.
- **Keep it short.** Self-guided should take ~10 minutes. Observed should take ~15-20 minutes. If the workflow has many steps, group related steps into combined tasks rather than asking about each one individually.
