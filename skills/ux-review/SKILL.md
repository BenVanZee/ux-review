---
name: ux-review
description: Reviews UI component and template code against UX principles from 6 research-backed books. Produces prioritized issue lists with file:line references and fix suggestions. Integrates bidirectionally with .interface-design/system.md.
---

# UX Review

Review UI code for usability, cognitive load, accessibility, and emotional design — grounded in 6 books.

## Scope

**Use for:** React/Vue/Svelte components, page templates, modals, forms, navigation, dashboards — any UI code.

**Not for:** Design tokens, CSS utility files, build config, backend code.

---

## Severity Levels

| Level | Meaning | Action |
|-------|---------|--------|
| **CRITICAL** | Actively harms usability, causes data loss, or blocks task completion | Must fix |
| **WARNING** | Degrades experience but user can still complete their task | Should fix |
| **SUGGESTION** | Improvement that would elevate the experience | Consider |
| **DELIGHT** | Opportunity to add personality and emotional engagement | Opportunity |

---

## The 6 Books

| Shorthand | Title | Author | Focus |
|-----------|-------|--------|-------|
| Krug | Don't Make Me Think | Steve Krug | Self-evident design, navigation, visual hierarchy |
| Rocket | Rocket Surgery Made Easy | Steve Krug | Common usability problems, quick fixes |
| Laws | Laws of UX | Jon Yablonski | Fitts's, Hick's, Jakob's, Miller's Law |
| Whalen | Design for How People Think | John Whalen | Cognitive psychology, mental models, perception |
| Tidwell | Designing Interfaces | Jenifer Tidwell | UI patterns for layout, nav, forms, actions |
| Walter | Designing for Emotion | Aarron Walter | Personality, delight, emotional engagement |

For deep dives, read the corresponding file in `references/`.

---

## Heuristics

### NAV — Navigation & Information Architecture

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| NAV-01 | **Users should always know where they are.** Current location must be visually distinct in navigation. | Krug, Tidwell | Nav items without active/current state styling; no breadcrumbs on deep pages; sidebar with no highlighted item |
| NAV-02 | **Navigation labels should be self-evident, not clever.** Users scan — they don't decode. | Krug | Nav labels that are ambiguous, branded jargon, or require domain knowledge without tooltips |
| NAV-03 | **Group related items and separate unrelated ones.** Proximity = relationship. | Tidwell, Whalen | Flat nav lists with 8+ ungrouped items; mixed concerns in the same nav section |
| NAV-04 | **Persistent navigation should be persistent.** Don't hide primary nav on interior pages. | Krug | Sidebar or header nav absent on some routes; conditional rendering of nav based on page |
| NAV-05 | **Every page needs a clear visual hierarchy.** Primary action > secondary content > tertiary info. | Krug, Whalen | Pages where all elements have equal visual weight; no size/weight/color differentiation between heading levels |
| NAV-06 | **Provide escape hatches.** Users should always be able to go back, cancel, or start over. | Tidwell | Modals without close buttons; multi-step flows without back buttons; no cancel on forms |
| NAV-07 | **Search should be findable and forgiving.** If the app has >20 items, search isn't optional. | Krug, Tidwell | Large data sets with no search/filter; search that requires exact matches; no empty state for zero results |

### COG — Cognitive Load & Complexity

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| COG-01 | **Reduce choices to reduce decision time (Hick's Law).** More options = longer decisions = more abandonment. | Laws | Dropdowns with 15+ unsorted options; action bars with 5+ buttons at same prominence; settings pages with everything visible |
| COG-02 | **Chunk information into groups of 5±2 (Miller's Law).** Working memory is limited. | Laws, Whalen | Forms with 10+ fields and no grouping; long unbroken lists; tables with 8+ columns visible at once |
| COG-03 | **Progressive disclosure — show only what's needed now.** Don't front-load complexity. | Tidwell, Whalen | All form fields visible at once in complex flows; advanced options not collapsed; every setting on one page |
| COG-04 | **Use recognition over recall.** Show options, don't make users remember them. | Whalen, Krug | Text inputs where dropdowns would work; instructions that reference hidden UI; empty inputs with no placeholder or example |
| COG-05 | **Consistent patterns reduce learning cost (Jakob's Law).** Users spend most of their time on other apps — match their expectations. | Laws | Custom UI patterns where standard ones exist (custom date picker vs. native, unusual toggle behavior); non-standard icon meanings |
| COG-06 | **Minimize page-to-page context switching.** Keep related actions on the same screen when possible. | Whalen | CRUD flows that navigate away unnecessarily; detail views that require navigating back to edit; modal → new page → modal chains |
| COG-07 | **Smart defaults reduce effort.** Pre-fill what you can predict. | Tidwell, Krug | Date pickers defaulting to epoch; country selectors with no default; forms that don't remember previous selections |

### VIS — Visual Hierarchy & Layout

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| VIS-01 | **Size and weight signal importance.** The most important element should be the most visually prominent. | Whalen, Krug | Primary actions smaller than secondary ones; page titles same size as body text; key metrics with no visual emphasis |
| VIS-02 | **Whitespace is not wasted space.** It groups, separates, and gives content room to breathe. | Tidwell, Whalen | Dense layouts with no spacing between sections; cards/panels with minimal padding; content touching container edges |
| VIS-03 | **Alignment creates order.** Inconsistent alignment signals sloppiness and increases scanning time. | Tidwell | Mixed left/center alignment on the same page; form labels inconsistently positioned; jagged right edges in card grids |
| VIS-04 | **Contrast guides attention.** Use color, weight, and size contrast intentionally — not decoratively. | Whalen | Multiple accent colors competing for attention; important actions in muted colors; decorative color with no meaning |
| VIS-05 | **Proximity implies relationship (Gestalt).** Items near each other are perceived as related. | Laws, Whalen | Submit button far from form fields; error messages distant from the field they reference; labels not visually connected to their inputs |
| VIS-06 | **Scan patterns matter — design for F-pattern or Z-pattern.** Users don't read, they scan. | Krug, Whalen | Important actions placed bottom-right where eyes don't go first; key info buried in paragraph text; CTAs below the fold with no visual pull |

### FRM — Forms & Input

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| FRM-01 | **Labels should be above or beside inputs, never as placeholders only.** Placeholders disappear on focus. | Tidwell, Krug | Inputs with only `placeholder` and no `<label>` element; labels that vanish when user types |
| FRM-02 | **Group related fields visually.** Address fields together, dates together, etc. | Tidwell | All fields in a flat single-column list with no visual grouping; related fields separated by unrelated ones |
| FRM-03 | **Show validation errors inline, next to the field.** Don't make users hunt for what went wrong. | Krug, Tidwell | Only top-of-form error summaries; toast-only error messages; error state on form but no per-field messages |
| FRM-04 | **Use the right input type for the data.** Dropdowns for constrained choices, text for free-form, date pickers for dates. | Tidwell | Free text inputs for data that has a fixed set of values; number inputs for phone numbers; generic text for email/URL |
| FRM-05 | **Indicate required fields clearly.** Don't make users submit to discover what's required. | Krug | Forms with no required indicators; asterisks with no legend; required fields only revealed by validation errors |
| FRM-06 | **Disable submit only when the reason is clear.** A disabled button with no explanation is a dead end. | Tidwell, Whalen | Disabled submit buttons with no tooltip or helper text explaining why; buttons that stay disabled with no visible validation state |
| FRM-07 | **Keep forms as short as possible.** Every field is friction. Ask only what you need right now. | Krug, Tidwell | Forms asking for info not needed at this step; optional fields presented as required; fields that could be auto-filled or deferred |

### FBK — Feedback & Error Handling

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| FBK-01 | **Every action should have visible feedback.** Users need to know something happened. | Whalen, Krug | Button clicks with no loading state, no toast, and no UI change; form submits that silently succeed or fail |
| FBK-02 | **Loading states should indicate progress, not just activity.** Spinners with no context cause anxiety. | Whalen | Bare spinner with no text; full-page loader blocking interaction; no skeleton screens for content areas |
| FBK-03 | **Error messages should be human-readable and actionable.** Say what happened and what to do. | Krug, Whalen | Raw API error messages shown to users; generic "Something went wrong" with no next step; error codes with no explanation |
| FBK-04 | **Confirmation before destructive actions.** Delete, remove, cancel — always confirm. | Tidwell | Delete buttons that act immediately; no confirmation dialog for destructive operations; destructive actions styled like normal actions |
| FBK-05 | **Success feedback should be proportional to effort.** A save after 30 seconds of work deserves more than a save after 1 click. | Walter, Whalen | Same generic toast for all success states; no distinction between routine saves and milestone completions |

### CON — Consistency & Patterns

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| CON-01 | **Same action, same appearance.** Delete buttons should look the same everywhere. Edit should look the same everywhere. | Laws, Krug | Destructive actions using different variants across pages; edit buttons that are sometimes icons, sometimes text, sometimes links |
| CON-02 | **Same data, same format.** Dates, phone numbers, currencies — pick a format and use it everywhere. | Krug | Dates shown as "Jan 1, 2025" in one place and "2025-01-01" in another; inconsistent phone number formatting |
| CON-03 | **Same component, same layout.** Cards should have the same structure across similar contexts. | Laws, Tidwell | Cards with title on top in one section and title inline in another; different spacing systems across similar components |
| CON-04 | **Interaction patterns should be predictable.** If clicking a row opens a detail view on one table, it should do so on all tables. | Laws | Some table rows are clickable, others aren't; some items open modals, similar items navigate to pages |
| CON-05 | **Terminology should be consistent.** Don't call it "Carrier" in nav and "Company" on the page. | Krug | Different labels for the same concept across pages; mixed terminology in nav vs. headers vs. form labels |

### ACC — Accessibility & Inclusivity

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| ACC-01 | **All interactive elements must be keyboard accessible.** Tab order, Enter/Space activation, Escape to close. | Tidwell | `onClick` on `<div>` or `<span>` without `role="button"` and `tabIndex`; custom components not handling keyboard events |
| ACC-02 | **Color must not be the only indicator.** Always pair color with text, icon, or pattern. | Tidwell, Laws | Status shown only by color dot with no label; error state indicated only by red border; success only by green background |
| ACC-03 | **Touch targets should be at least 44x44px.** Small targets cause misclicks and frustrate users (Fitts's Law). | Laws, Tidwell | Icon buttons smaller than `h-8 w-8`; clickable text with no padding; close buttons that are tiny `x` characters |
| ACC-04 | **Images and icons need text alternatives.** Screen readers need context. | Tidwell | `<img>` without `alt`; icon-only buttons without `aria-label`; decorative icons without `aria-hidden="true"` |
| ACC-05 | **Sufficient color contrast.** Text must be readable against its background (WCAG AA: 4.5:1 for normal text, 3:1 for large text). | Laws | Light gray text on white backgrounds; colored text on colored backgrounds; disabled states that are invisible |

### DLT — Delight & Personality

| ID | Heuristic | Source | Code Signals |
|----|-----------|--------|-------------|
| DLT-01 | **Success moments should celebrate, not just confirm.** "Saved" is functional; celebration is pleasurable. | Walter | Success toasts with only generic text; no visual distinction between routine saves and milestone completions |
| DLT-02 | **Microcopy should have personality and warmth.** Empty states, tooltips, and helper text are opportunities for voice. | Walter | Empty states saying only "No items found"; generic placeholder text; robotic confirmation messages |
| DLT-03 | **Error recovery should be empathetic, not clinical.** Acknowledge frustration, guide forward. | Walter, Whalen | Error messages using technical jargon; no reassurance or suggested next steps; blame-the-user tone |
| DLT-04 | **Reduce anxiety at high-stakes moments.** Destructive actions, payments, submissions — provide reassurance. | Walter, Whalen | Delete confirmations with no "what happens" context; no undo options; irreversible actions without safety nets |
| DLT-05 | **Reward completion and progress.** Users should feel momentum. | Walter | Multi-step flows with no progress indicator; completed tasks that just disappear; no visual completion state |
| DLT-06 | **Surprise with thoughtful details in low-stakes moments.** Subtle animations, contextual humor, smart defaults. | Walter | Every interaction is purely utilitarian; no transition animations; no contextual intelligence (smart defaults, remembered preferences) |

---

## Review Process

### 1. Identify Target Files

**From arguments:**
- `<path>` — single file or directory
- `--changed` — `git diff --name-only` (uncommitted changes)
- `--staged` — `git diff --staged --name-only`
- `--category=<name>` — focus review on one heuristic category

**From defaults (no args):**
- Glob for common UI paths: `src/pages/**/*.tsx`, `src/components/**/*.tsx`, `app/**/*.tsx`, `resources/views/**/*.blade.php`, `app/views/**/*.erb`

**Always exclude:** `components/ui/` (shadcn primitives), `node_modules/`, `dist/`, `build/`, `.next/`

### 2. Load Design System

Read `.interface-design/system.md` from the project root (or `../.interface-design/system.md` for monorepos). Parse for:
- Stack (React, Vue, etc.)
- Spacing tiers and base unit
- Typography rules (allowed sizes, weights)
- Color tokens and semantic colors
- Component patterns (Card, Table, Button, etc.)
- Don'ts list
- Depth strategy (borders vs. shadows)
- Icon sizes
- Border radius rules

If no system.md found, note it in the output and skip design system checks.

### 3. Load Workflows

Read all files in `docs/workflows/` from the project root (exclude `docs/workflows/test-scripts/`). Parse for:
- Workflow names and who performs them
- Step sequences and which screens/routes are involved
- Known pain points and what matters most
- Success criteria

If no workflow docs found, print a one-line note and continue with standard heuristic review:

```
No workflow docs found in docs/workflows/. Run /ux-review:build-workflows to add domain context.
```

### 4. Review Each File

For each file:

1. **Identify component type** — page, modal, form, list/table, navigation, dashboard, detail view, empty state
2. **Apply relevant heuristic subset** — not all 48 apply to every component type:
   - **Pages:** NAV, VIS, COG, CON, DLT
   - **Modals:** NAV-06, COG-02, COG-03, FRM (all), FBK, DLT-04
   - **Forms:** FRM (all), FBK, COG-02, ACC, DLT-03
   - **Tables/Lists:** VIS, COG-01, COG-02, CON, ACC-01, NAV-07
   - **Navigation:** NAV (all), CON, ACC-01
   - **Empty states:** DLT-02, VIS-01, NAV-07
3. **Look for code signals** — the specific patterns listed in each heuristic
4. **Record findings** with severity, file:line reference, description, book citation, and fix suggestion

### 5. Workflow Alignment Check

If workflow docs were loaded in step 3, apply these checks to each reviewed file:

- **Does this screen serve the workflow it belongs to?** If a workflow says a step is "verify invoices," does the screen make that step fast and obvious?
- **Is the workflow's critical path the UI's happy path?** Are the most common steps the easiest to reach, or is the user fighting the layout?
- **Do known pain points have visible UI causes?** If the workflow doc says "users waste time switching between tabs," does the code confirm the data lives on separate pages?
- **Are workflow steps reflected in navigation?** Does the app's structure match the user's mental model of the workflow?
- **Does step-to-step flow feel natural?** Can the user move from one workflow step to the next without unnecessary navigation, backtracking, or context switching?

Record findings with the `WF-XX` prefix.

### 6. Cross-File Consistency Check

After individual reviews, check across files:
- **CON-01:** Same action buttons using the same variant/style?
- **CON-02:** Same data type formatted the same way?
- **CON-03:** Same component structure used consistently?
- **CON-04:** Same interaction pattern (click → modal vs. click → navigate)?
- **CON-05:** Same terminology for the same concepts?

### 7. Design System Bidirectional Check

**DS-CODE (code violates system):**
Compare reviewed code against parsed system.md rules. Flag:
- Spacing not on the defined grid
- Typography violating size/weight rules
- Colors not in the palette
- Components not matching defined patterns
- Depth violations (shadows where system says borders-only)
- Don'ts list violations

**DS-UX (system conflicts with UX principle):**
Check if any system.md rules conflict with UX heuristics. Examples:
- System's smallest icon size on interactive elements → may violate ACC-03 (44px targets)
- System disallowing all shadows → may hurt VIS hierarchy for elevated elements
- System mandating a specific text size → may cause readability issues in certain contexts

For DS-UX findings, propose a specific edit to system.md.

### 8. Output

Use the output format defined below. Sort issues by severity: CRITICAL > WARNING > SUGGESTION > DELIGHT.

---

## Output Format

```
## UX Review: [scope description]

Design System: [Loaded from .interface-design/system.md | Not found]
Workflows: [Loaded X from docs/workflows/ | Not found]
Files Reviewed: [count]
Issues: [X] critical, [X] warnings, [X] suggestions, [X] delight opportunities

---

### CRITICAL

**[ID] Short description**
`file/path.tsx:line`
Description of the issue — what's wrong and why it matters for the user.
> *Book Title, Chapter/Section reference*
**Fix:** Specific code-level suggestion.

---

### WARNING

**[ID] Short description**
`file/path.tsx:line`
Description.
> *Source*
**Fix:** Suggestion.

---

### SUGGESTION

**[ID] Short description**
`file/path.tsx:line`
Description.
> *Source*
**Fix:** Suggestion.

---

### DELIGHT OPPORTUNITIES

**[ID] Short description**
`file/path.tsx:line`
Description — what opportunity exists and why it would improve the experience.
> *Source*
**Fix:** Suggestion.

---

### DESIGN SYSTEM NOTES

**[DS-CODE] Description** — `file:line`
What the system says vs. what the code does. Fix suggestion.

**[DS-UX] Description**
What the system says vs. what UX principle recommends.
Proposed change to system.md.

---

### WORKFLOW ALIGNMENT

**[WF-XX] Short description of the misalignment**
`file/path.tsx:line`
What the workflow expects vs. what the UI provides.
> *Source*
**Fix:** Specific suggestion.

---

### CROSS-FILE CONSISTENCY

**[CON-XX] Description**
Files compared: `file1.tsx`, `file2.tsx`
What's inconsistent and which pattern should win.
```

If a section has no findings, omit it entirely. If the review is clean, say so:

```
## UX Review: [scope]

No critical issues found. [X] suggestions for improvement.
```

---

## Commands

- `/ux-review:review` — Main review against all heuristics (workflow-aware if docs exist)
- `/ux-review:deep` — Deep dive using one book's full reference
- `/ux-review:build-workflows` — Guided interview to document business workflows
- `/ux-review:user-script` — Generate usability test scripts from documented workflows

## Deep Dives

For detailed analysis using a specific book's lens, see the reference files:
- `references/dont-make-me-think.md` — Krug's usability principles
- `references/rocket-surgery.md` — Common usability problems and quick fixes
- `references/laws-of-ux.md` — Psychological laws applied to UI
- `references/design-for-how-people-think.md` — Cognitive psychology and mental models
- `references/designing-interfaces.md` — UI patterns and pattern language
- `references/designing-for-emotion.md` — Emotional design and personality
