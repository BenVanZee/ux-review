# Rocket Surgery Made Easy — Steve Krug

A deep-dive reference for UX code review through the lens of Steve Krug's guide to finding and fixing usability problems. Use this when running `/ux-review:deep rocket`.

---

## Core Thesis

You don't need a usability lab, a research budget, or weeks of planning to find usability problems. **Most serious usability issues are hiding in plain sight** — they're the things that make users hesitate, squint, or reach for the back button. The hard part isn't finding them; it's being willing to look.

Krug's key insight: **"Testing one user early in the project is better than testing 50 near the end."** Applied to code review: catching one UX issue before merge is better than getting 50 bug reports after launch.

---

## The Top Usability Problems (And How They Show Up in Code)

### 1. Users Don't Know Where to Click

**The problem:** The page has content, but users can't figure out what to do. There's no clear call to action, or too many competing actions, or interactive elements don't look interactive.

**Code signals:**
- Action buttons buried below the fold or in unexpected locations
- Multiple buttons with equal visual weight — no primary/secondary distinction
- Clickable elements using `<div onClick>` without `cursor-pointer`, hover states, or focus indicators
- Links styled identically to surrounding text
- Icon-only actions without labels or tooltips

**Quick fixes:**
- Make one action visually dominant (larger, colored, positioned prominently)
- Add `cursor-pointer` and hover states to all clickable elements
- Use button variants: primary for the main action, outline/ghost for secondary
- Add text labels to icon buttons, or at minimum `aria-label` and `title`

### 2. Users Can't Find Things

**The problem:** The information is on the page, but users can't locate it. Poor visual hierarchy, no grouping, or unexpected placement.

**Code signals:**
- All text at the same size and weight — no hierarchy
- Related information scattered across different sections
- Key data points buried in paragraphs instead of highlighted
- Search functionality missing on pages with many items
- Filters that are hidden or collapsed by default on data-heavy pages

**Quick fixes:**
- Create clear heading hierarchy (title > section header > body)
- Group related items visually with cards, borders, or spacing
- Pull key data into prominent positions (top of card, bold, larger)
- Add search/filter when there are more than ~15 items

### 3. Users Get Lost

**The problem:** Users don't know where they are in the app, how they got there, or how to get back.

**Code signals:**
- No active state on navigation items
- Missing breadcrumbs on deeply nested pages
- Modal → page → modal flows that lose context
- No back button or back link on detail pages
- Page titles that don't match the nav item that was clicked

**Quick fixes:**
- Add conditional active styling to nav items based on current route
- Add breadcrumbs for pages 2+ levels deep
- Ensure modals don't navigate away — or if they do, provide clear way back
- Match page titles to navigation labels exactly

### 4. Forms Are Confusing

**The problem:** Users struggle to fill out forms — they don't know what's required, what format to use, or what went wrong.

**Code signals:**
- No visible `<label>` elements — only `placeholder` attributes
- Required fields with no asterisk or other indicator
- Validation that only runs on submit, not inline
- Error messages that say "Invalid input" instead of explaining the requirement
- Date/phone/currency fields with no format hint
- Disabled submit buttons with no explanation of what's missing

**Quick fixes:**
- Add visible labels above every input (not just placeholder)
- Mark required fields with `*` and add a legend ("* Required")
- Add inline validation on blur for critical fields
- Write specific error messages: "Phone number must be 10 digits" not "Invalid"
- Add format hints: `placeholder="(555) 123-4567"` combined with a visible label
- Add helper text near disabled submit explaining what's needed

### 5. Users Don't Get Feedback

**The problem:** Users take an action and nothing visibly happens. Or something happens but they're not sure what.

**Code signals:**
- Form submission with no loading state (no spinner, no disabled button)
- API calls with no toast, alert, or UI change on success
- Buttons that trigger async operations but remain in their default state
- Error responses that are caught but produce no user-facing message
- Save operations with no confirmation

**Quick fixes:**
- Add loading spinner to buttons during async operations
- Show success toast or message after every mutation
- Show error toast with human-readable message on failure
- Disable the triggering button while the operation is in progress
- For important operations, update the UI to reflect the change immediately (optimistic update)

---

## Krug's "Do This, Not That" for Code Review

### Don't: Assume Users Read

**Code smell:** Long instruction text, info paragraphs above forms, explanatory modals that assume people will read them.

**Instead:** Make the UI self-explanatory. If you need instructions, the UI isn't clear enough. Exception: truly complex domain-specific workflows where a brief inline tip is genuinely helpful.

### Don't: Assume Users Know Where They Are

**Code smell:** Pages that rely on the URL bar for orientation. Detail pages with no context about the parent. Modals that don't show what record they're editing.

**Instead:** Every page/modal should show context: whose record is this? What section of the app are we in? What was the last page?

### Don't: Make Users Remember Things

**Code smell:** Multi-step flows where information entered in step 1 isn't visible in step 3. Forms that don't pre-fill known values. Error messages that reference something the user can't see.

**Instead:** Show relevant context at every step. Pre-fill what you know. Inline errors next to the field they reference.

### Don't: Hide the Primary Action

**Code smell:** "Submit" button at the bottom of a long scrolling form that's not visible without scrolling. Primary action button in a dropdown menu. Key action only accessible through right-click or keyboard shortcut.

**Instead:** Primary actions should be visible without scrolling. Sticky footers for long forms. Clear visual hierarchy among action buttons.

---

## The "Fix It" Prioritization

Krug's approach to fixing usability problems, applied to code review severity:

### Fix These First (CRITICAL)
- **Can't complete the task.** Form can't be submitted, navigation is broken, key actions are hidden or non-functional.
- **Data loss risk.** No confirmation before destructive actions. No save indicator. No undo.
- **Feedback black holes.** Actions that produce no visible result — user has no idea if it worked.

### Fix These Next (WARNING)
- **Confusion delays.** Users can complete the task but waste time figuring out how. Unclear labels, poor hierarchy, missing context.
- **Error recovery is painful.** Errors happen but recovery requires starting over, or error messages don't help.
- **Navigation is disorienting.** Users can eventually find things but get lost along the way.

### Fix When Possible (SUGGESTION)
- **Polish and refinement.** Better microcopy, clearer status indicators, more informative empty states.
- **Scannability improvements.** Better visual hierarchy, more whitespace, clearer grouping.
- **Reducing cognitive load.** Fewer choices presented at once, smarter defaults, progressive disclosure.

---

## Applying Rocket Surgery to Component Types

### Page Components
1. Apply the trunk test — can you identify site, page, section, options, and location?
2. Check the visual hierarchy — is there one clear primary action?
3. Verify navigation shows current location

### Form Components
1. Check every input for a visible label
2. Check for required field indicators
3. Look for inline validation
4. Verify error message specificity
5. Check submit button label (specific verb, not "Submit")

### Table/List Components
1. Check for empty state
2. Verify sort/filter/search if >15 items
3. Check for hover states on interactive rows
4. Verify column headers are clear (no ambiguous abbreviations)

### Modal Components
1. Check for close button and escape key handler
2. Verify title describes the action
3. Check action button labels (specific, not "OK")
4. Look for loading/error states on modal actions
