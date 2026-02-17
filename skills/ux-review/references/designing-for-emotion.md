# Designing for Emotion — Aarron Walter (A Book Apart)

A deep-dive reference for UX code review through the lens of Aarron Walter's guide to emotional design and product personality. Use this when running `/ux-review:deep emotion`.

---

## Core Thesis

Usable isn't enough. **Usable is the baseline.** If all your app does is function correctly, you've met the minimum — you haven't earned loyalty, delight, or word of mouth. People don't love tools that merely work. They love tools that make them **feel something**.

Walter builds on Maslow's hierarchy of needs to create a **hierarchy of user needs:**

1. **Functional** — Does it work?
2. **Reliable** — Can I depend on it?
3. **Usable** — Can I use it without effort?
4. **Pleasurable** — Does it make me feel good?

Most software gets stuck at level 3. This book is about reaching level 4.

**For internal tools (like DOTTie, FM22, Helix):** This matters even more. Your employees use these tools 8 hours a day. A tool that's merely usable is one they tolerate. A tool with personality is one they enjoy — and enjoyment reduces errors, increases adoption, and makes people's workdays better.

---

## Emotional Design Principles — Applied to Code Review

### 1. Product Personality

**The principle:** Your product should have a personality — not a brand, not a color scheme, but a **voice and character** that comes through in microcopy, empty states, error messages, and success moments.

**What to look for in code:**

- **Generic microcopy everywhere.** Empty states that say "No items found." Confirmation dialogs that say "Are you sure?" Success toasts that say "Saved successfully." These are functional, not personal.
- **No voice.** Read all the visible text strings in a component. Could they belong to any app? If you can't tell what product this is from the copy alone, there's no personality.
- **Robotic error messages.** "Validation failed: field is required." "Error 500." "Request timed out." These are developer messages, not human messages.
- **No contextual awareness.** Every empty state uses the same message. Every success uses the same toast. The app doesn't acknowledge what the user just accomplished or what they might want to do next.

**What good looks like:**
```
// Generic (no personality):
"No work orders found."

// With personality (warm, helpful):
"No work orders yet. Create one to get started."

// With personality (acknowledging context):
"All caught up! No pending work orders need attention."
```

**Review approach:** Read every user-facing string literal in the reviewed files. For each, ask: "Is this generic or does it have a voice?"

### 2. Hierarchy of Emotional Response

Walter describes three levels of emotional response:

- **Visceral** — Immediate, gut reaction to visual appearance. "This looks professional" or "This looks broken."
- **Behavioral** — Response during use. "This feels smooth" or "This is frustrating."
- **Reflective** — After use. "I trust this tool" or "I dread opening this app."

**What to look for in code:**

**Visceral (visual craft):**
- Consistent spacing, alignment, and typography → builds trust
- Broken layouts, mixed styles, unstyled states → erodes trust
- Attention to small details (border radius consistency, icon sizing) → signals care

**Behavioral (interaction quality):**
- Smooth transitions and animations → feels polished
- Instant feedback on every action → feels responsive
- Graceful error handling → feels reliable
- No feedback, jarring transitions, lost state → feels broken

**Reflective (overall impression):**
- Completion moments that acknowledge effort → positive memory
- Consistent, predictable patterns → trust
- Thoughtful empty states and error recovery → empathy

### 3. Designing for Delight

**The principle:** Delight isn't decoration — it's the thoughtful details that show someone cared. It lives in the seams of the experience: the moments between major actions.

**Opportunities to look for in code:**

#### Success Celebrations
The most common missed opportunity. User completes a significant action — creating a work order, finishing a multi-step flow, completing a batch operation — and the app responds with a generic toast.

**Code signals:**
- `toast("Created successfully")` or `toast.success("Saved")` — functional, not celebratory
- No distinction between saving a typo fix and completing a 20-field form
- Multi-step flows that end with a redirect and no acknowledgment

**What good looks like:**
- Success messages that reference what was created ("Work order WO-2024-0047 created")
- Milestone completions with distinct visual treatment (not just another toast)
- Next-step suggestions ("View work order" / "Create another")

#### Empty State Personality
Empty states are prime real estate for personality. They're the first thing new users see and a regular occurrence for power users filtering to zero results.

**Code signals:**
- `"No items found"` or `"No data"` — minimum viable empty state
- No icon or illustration — just text
- No action button to create the first item
- Same empty state message everywhere

**What good looks like:**
- Contextual messages that acknowledge the specific situation
- An icon that sets the mood (not just a generic circle-slash)
- A clear call to action
- Different messages for "no items yet" vs. "no results for this filter"

#### Microcopy Warmth
Tooltips, helper text, placeholder text, confirmation messages — these are opportunities for voice.

**Code signals:**
- Tooltips that just repeat the label ("Delete: Deletes this item")
- Helper text that's overly technical ("Format: YYYY-MM-DD")
- Placeholder text that's generic ("Enter value...")
- Confirmation messages with no personality ("Operation completed")

**What good looks like:**
- Helper text that's genuinely helpful ("We'll use this to calculate quarterly taxes")
- Placeholders that provide context ("e.g., Acme Trucking LLC")
- Confirmations that reference the specific action and its result

#### Loading State Personality
Instead of bare spinners, loading states can communicate something useful or charming.

**Code signals:**
- `<Loader2 className="animate-spin" />` with no text — tells the user nothing
- Full-page loaders that block all interaction
- No skeleton screens for content areas

**What good looks like:**
- Loading messages that describe what's happening ("Fetching carrier data...")
- Skeleton screens that show the shape of incoming content
- Progress indicators for multi-step operations

### 4. Error Recovery as Empathy

**The principle:** When things go wrong, the app's response reveals its character. Clinical error messages feel like the app is blaming the user. Empathetic error messages feel like the app is helping the user.

**What to look for in code:**

- **Technical error messages shown to users.** Stack traces, HTTP status codes, raw API error messages. These are developer artifacts, not user communication.
- **Blame-the-user tone.** "Invalid input." "You must provide a value." "Incorrect format." These phrases imply the user did something wrong — even when the UI didn't prevent the error.
- **No recovery guidance.** Error messages that say what went wrong but not what to do about it. "Upload failed" without "Try a smaller file or a different format."
- **No reassurance.** Errors during important operations (saving data, submitting forms) with no indication that existing data is safe. "Error saving" without "Your previous changes are still saved."

**Error message template:**
1. **What happened** (in plain language, not technical jargon)
2. **It's not your fault** (reassurance, when appropriate)
3. **What to do** (specific next step)
4. **Your data is safe** (when applicable)

```
// Bad:
"Error 422: Validation failed"

// Better:
"We couldn't save the work order. The MC number format looks different
than expected — try using just the numbers (e.g., 123456)."

// Best (with reassurance):
"Hmm, that MC number doesn't look right. No worries — everything else
you entered is saved. Try just the numbers, like 123456."
```

### 5. Anxiety Reduction

**The principle:** Users experience anxiety at high-stakes moments — destructive actions, irreversible submissions, payment, bulk operations. The app should reduce anxiety, not amplify it.

**What to look for in code:**

- **Destructive confirmations with no context.** "Are you sure you want to delete this?" doesn't explain what will happen. Will related items be deleted? Can this be undone?
- **No undo option.** Destructive actions that are immediately permanent. Even a 5-second "Undo" toast is better than nothing.
- **Irreversible actions without safety nets.** Bulk operations, status changes, submissions — with no way to reverse if the user made a mistake.
- **No progress saving.** Long forms that don't auto-save or warn before navigating away. Users fear losing 10 minutes of data entry.
- **Ambiguous button pairs.** "OK" / "Cancel" on a destructive confirmation — which one is the safe option? Use specific verbs: "Delete Work Order" / "Keep Work Order."

### 6. Rewarding Completion

**The principle:** Users should feel a sense of accomplishment when they complete significant tasks. The app should reward completion proportionally to effort.

**What to look for in code:**

- **No completion state.** Multi-step flows that just redirect to a list page. Complex form submissions with a brief toast. Batch operations that end silently.
- **Generic success for everything.** Same toast for saving a quick edit and completing a 30-minute workflow. The response should match the magnitude of the effort.
- **No "what next?" guidance.** After completing a task, the user is left on a page with no obvious next action.
- **Completed tasks that just disappear.** Items that vanish from a list after being marked complete — the user doesn't get to see their progress.

---

## Emotional Design Review Checklist

For each component reviewed, ask:

| Question | If No → Heuristic |
|----------|-------------------|
| Does the success message reference what was done specifically? | DLT-01 |
| Do empty states have personality and guide the user? | DLT-02 |
| Do error messages show empathy and provide recovery steps? | DLT-03 |
| Are high-stakes moments softened with context and reassurance? | DLT-04 |
| Do multi-step flows show progress and reward completion? | DLT-05 |
| Are there any thoughtful details in low-stakes moments? | DLT-06 |

---

## The Voice Test

Read all user-facing text in the reviewed code — labels, button text, toasts, empty states, error messages, helper text. Then answer:

1. **Could this text belong to any app?** If yes, it has no personality.
2. **Would you know what product this is from the text alone?** If no, there's no voice.
3. **Does the text acknowledge the user's context?** (What they just did, what they might need next)
4. **Is there warmth?** Not fake cheerfulness — genuine care. "We're here to help" warmth, not "Awesome!!! Great job!!!" warmth.

Internal tools deserve voice too. Your employees deserve software that treats them as humans, not data entry machines.

---

## Severity Guide for Emotional Design

| Finding | Severity |
|---------|----------|
| Error messages that are raw API responses or stack traces | WARNING |
| Destructive actions with no consequence explanation | WARNING |
| Multi-step flows with no progress indicator | WARNING |
| Generic empty states with no guidance | DELIGHT |
| Generic success messages that could be warmer | DELIGHT |
| Microcopy that could have more personality | DELIGHT |
| Missing subtle animations or transitions | DELIGHT |
| No personality anywhere in the component | DELIGHT |

Note: Most emotional design findings are DELIGHT severity — they're opportunities to elevate, not issues that harm usability. The exceptions are error handling (WARNING) and anxiety-inducing patterns (WARNING), which actively harm the experience.
