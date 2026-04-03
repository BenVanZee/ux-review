---
name: ux-review:build-workflows
description: Guided interview to thoroughly document a business workflow. Produces structured workflow docs that serve as anchor specs for feature development and make future UX reviews workflow-aware.
---

# Build Workflows

Document a business workflow through a thorough guided conversation. The output is a spec-grade workflow document that serves as the anchor for feature development, page design, and UX reviews.

## Usage

```
/ux-review:build-workflows                    # Asks which workflow to document
/ux-review:build-workflows morning-funding    # Names the workflow upfront
```

## Process

### 1. Check Existing Workflows

Read `docs/workflows/` in the project root. If workflows already exist, show a quick summary so the user knows what's already documented and can avoid duplicates.

### 2. Interview

Ask questions **one at a time**. Wait for each answer before asking the next. The interview has three phases: context, walkthrough, and depth.

#### Phase 1: Context

1. **What is this workflow?** — Ask for a name and a one-sentence description of what it accomplishes.

2. **Does this workflow happen in an existing app, or are we describing a business process for something that will be built?** This determines how the rest of the interview works:
   - **Existing app:** Follow-ups reference specific screens, routes, and current UI. Browser validation is offered at the end.
   - **Business process (no app yet):** Follow-ups focus on what information is needed, what tools/systems are involved (spreadsheets, phone calls, other software, paper), and what the ideal experience would look like. No screen routes, no browser validation.

3. **Who performs it?** — Role or specific person (CSA, account manager, etc.). If multiple roles are involved, identify each and where they hand off.

4. **When/what triggers it?** — "Every morning", "When a client submits a schedule", "When a new carrier calls", etc.

5. **How often, and how many?** — Frequency (daily, weekly, on-demand) AND volume (how many per day/week/month). "We process about 40 schedules every morning" is very different from "we get maybe 2 a week." Volume drives design decisions like bulk actions, search, and pagination.

6. **What has to be true before this workflow can start?** — Prerequisites and preconditions. "Carrier must already be in the system." "Client must have submitted a schedule." "Invoices must be verified." These become validation rules and guards in the system.

#### Phase 2: Walkthrough

7. **Walk me through the steps.** — Let the user describe the flow naturally. For each step, ask follow-up questions to fill in the complete picture:

   **For every step:**
   - **What information do you need to see** at this step to make a decision or take action?
   - **What data do you input** at this step? (fields, selections, uploads)
   - **Are there decision points?** If yes, what determines the path? (business rules, data conditions)
   - **What are the hard rules?** Business rules that must ALWAYS be true. ("Can't fund more than 90% of invoice value." "Must have a signed NOA on file." "Duplicate docket numbers are rejected.") These are the most critical spec details — probe specifically for them.
   - **What does this step produce?** — Does it create a record, send an email, generate a document, update a status, trigger a notification?

   **If existing app:**
   - Clarify which screens/pages are involved ("What screen are you on?")

   **If business process:**
   - What tools or systems are currently used at this step (email, spreadsheet, phone, another app, paper form, etc.)
   - What would be ideal — what do you wish you could do instead?

   **Fill gaps aggressively:**
   - "What happens between [step A] and [step B]?"
   - "Is there anything you check or verify before moving on?"
   - "Does anyone else touch this before the next step?"

8. **Are there handoffs?** — Does work pass between people during this workflow? Where does one person's part end and another's begin? What information gets passed? How does the next person know it's their turn? (Email? They check a queue? Someone walks over?)

#### Phase 3: Depth

9. **What does success look like?** — How do you know this workflow completed correctly? What's the end state?

10. **What does this workflow produce?** — Concrete outputs: a funded invoice, a filed report, a PDF document, an email confirmation, an updated record, a notification to someone. Be specific.

11. **What goes wrong?** — Probe deeply here. Don't accept a single answer. Ask:
    - "What's the most common error or problem?"
    - "What happens when [data from step X] is missing or wrong?"
    - "Has there ever been a really bad failure? What happened?"
    - "What workarounds do people use when the normal process breaks down?"
    - "Are there steps where people frequently make mistakes?"

12. **Are there timing constraints or SLAs?** — "Must be done by 2pm." "Client expects a response within 1 hour." "Quarterly filing deadline is the last day of the month." Time pressure shapes which steps need to be fast vs. thorough.

13. **What matters most?** — Speed? Accuracy? Auditability? Compliance? Where does the pain concentrate? If you could only fix one thing about this workflow, what would it be?

### Interview Technique

**Ask for specific examples.** "Can you walk me through the last time you did this?" is better than "How does this usually work?" Specific examples surface edge cases and real behavior that general descriptions miss.

**Probe exceptions.** After each step, ask: "What happens when that's NOT the case?" or "What if [normal assumption] isn't true?" Exceptions reveal business rules.

**Ask "why" to surface hidden rules.** When the user says "we always check X before Y," ask "Why? What happens if you don't?" The answer is usually a business rule that needs to be enforced in software.

**Don't accept vague answers.** "We review the schedule" → "What specifically are you looking at? What makes a schedule good or bad? What would make you reject one?" Push until the answer is specific enough that a developer could implement it.

**Quantify when possible.** "It takes a long time" → "How long? 5 minutes? An hour?" "We get a lot of these" → "How many per day?" Numbers turn pain points into prioritized requirements.

### 3. Write the Workflow Doc

Create `docs/workflows/<workflow-name>.md` (kebab-case filename). Use this format:

```markdown
# <Workflow Name>

**Performed by:** <role or person — list all roles if multiple>
**Trigger:** <what starts this workflow>
**Frequency:** <how often — daily, on-demand, quarterly, etc.>
**Volume:** <how many per day/week/month>
**Context:** <existing app name OR "Business process — no app yet">
**Success:** <what a successful completion looks like>

## Prerequisites
- <what must be true before this workflow can start>
- <required data, prior steps, system state>

## Steps

1. **<Step name>** — <description>
   - Current tool: <app screen/route, spreadsheet, phone call, email, paper, etc.>
   - What matters: <what the user cares about at this step>
   - Information needed: <what the user needs to SEE or KNOW to proceed>
   - Inputs: <data the user enters, selects, or communicates>
   - Decisions: <branching logic, if any — what determines the next step>
   - Produces: <what this step creates — record, email, document, status change>
   - Business rules: <hard constraints that must be enforced at this step>

2. **<Step name>** — <description>
   - Current tool: <app screen/route, spreadsheet, phone call, email, paper, etc.>
   - What matters: <what the user cares about at this step>
   - Information needed: <what the user needs to SEE or KNOW to proceed>
   - Inputs: <data the user enters, selects, or communicates>
   - Decisions: <branching logic, if any — what determines the next step>
   - Produces: <what this step creates — record, email, document, status change>
   - Business rules: <hard constraints that must be enforced at this step>

## Handoffs
<If multiple people are involved: who hands off to whom, what information passes, how the next person knows it's their turn>

## Business Rules
<Consolidated list of all hard rules surfaced during the interview — these become validation logic and guards in the system>

## Outputs
<What this workflow produces when complete — documents, records, emails, notifications, status changes>

## Timing & SLAs
<Deadlines, time pressures, expected turnaround times>

## Pain Points
- <known frustrations, workarounds, bottlenecks — with specifics and quantities where possible>

## What Matters Most
<1-2 sentences on the priority — speed, accuracy, auditability, etc. The one thing to fix first.>

## Validation Notes
<!-- Added by browser walkthrough, if performed -->
```

Create the `docs/workflows/` directory if it doesn't exist.

After writing, show the user a summary of:
- Number of steps documented
- Key business rules captured
- Handoffs identified
- Major pain points
- Any gaps or open questions that still need answers

### 4. Optional Browser Validation

**Skip this step entirely if the workflow is a business process with no existing app.**

For existing-app workflows, after saving the doc, ask:

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
- **Don't assume screen paths.** If the user doesn't know the exact route, leave the Current tool field as `<unknown>` — it can be filled in during browser validation or later.
- **Use the user's language.** Match their terminology in the doc, not generic UX terms.
- **Create the directory** `docs/workflows/` if it doesn't exist.
- **Business rules are gold.** The most valuable output of this interview is the business rules — the hard constraints that must be enforced. Probe for them at every step. If the user says "we check X," ask what the rule is. If they say "we can't do Y," document exactly why and what the threshold is.
- **Don't settle for vague.** Every step should be specific enough that a developer who has never seen this workflow could build it. If a step is still vague after probing, flag it as an open question in the doc rather than leaving it ambiguous.
- **Suggest related workflows.** After completing the doc, note any adjacent workflows that were mentioned but not documented, and suggest the user document those next.
