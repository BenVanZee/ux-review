# Laws of UX — Jon Yablonski

A deep-dive reference for UX code review through the lens of Jon Yablonski's collection of psychological principles applied to interface design. Use this when running `/ux-review:deep laws`.

---

## Core Thesis

UX design isn't subjective — it's grounded in **psychological principles** that describe how humans perceive, process, and interact with information. These aren't guidelines or suggestions; they're cognitive realities. Ignoring them doesn't make users adapt — it makes them struggle.

Yablonski collects 20+ laws and principles from psychology and applies them directly to interface design. For code review, the most actionable laws are the ones that have clear, detectable code signals.

---

## The Laws — Applied to Code Review

### Fitts's Law

**"The time to acquire a target is a function of the distance to and size of the target."**

Translation: Small, distant click targets are hard to hit. Large, nearby ones are easy.

**What to look for in code:**
- **Tiny click targets.** Icon-only buttons with `w-4 h-4` or smaller and no surrounding padding. Close buttons (`×`) rendered as small text. Action links with no padding.
- **Touch target size.** On any touchscreen-capable interface, interactive elements should be at least 44×44px. Look for buttons with `h-6`, `h-7`, or icon buttons with tight dimensions.
- **Target spacing.** Adjacent clickable elements with no gap between them cause misclicks. A row of action icons with `gap-1` or no gap.
- **Important actions far from cursor context.** Confirmation dialogs where "Cancel" is far from the triggering element. Submit buttons at the bottom of long forms with no sticky positioning.

**Code-level checks:**
```
# Look for small interactive elements
h-4 w-4.*onClick
h-5 w-5.*onClick
h-6.*onClick
# Look for buttons without enough padding
p-0.*onClick
p-1.*onClick
```

**Severity guide:**
- Icon button < 32px with no padding expansion → WARNING
- Interactive element < 24px → CRITICAL (nearly impossible to tap on mobile)
- Important actions placed far from trigger context → SUGGESTION

---

### Hick's Law

**"The time it takes to make a decision increases with the number and complexity of choices."**

Translation: More options = slower decisions = more user frustration and abandonment.

**What to look for in code:**
- **Dropdown menus with many options.** `<Select>` or dropdown components rendering 15+ items without search, filtering, or categorization.
- **Action bars with too many buttons.** More than 3-4 buttons in a single action area, all at the same visual prominence.
- **Navigation with too many top-level items.** Sidebar or header with 8+ ungrouped navigation items.
- **Settings pages showing everything at once.** All configuration options visible simultaneously with no tabs, sections, or progressive disclosure.
- **Form fields presented all at once.** Complex forms (10+ fields) with no step-by-step or sectioned approach.

**Severity guide:**
- 15+ unsorted dropdown options → WARNING
- 5+ equally-prominent action buttons → WARNING
- 10+ form fields with no grouping → WARNING
- Settings page with 20+ options and no categorization → CRITICAL

---

### Jakob's Law

**"Users spend most of their time on other sites/apps. They prefer your site to work the same way as all the other sites they already know."**

Translation: Don't reinvent standard patterns. Match user expectations from the apps they already use.

**What to look for in code:**
- **Custom implementations of standard patterns.** A hand-built date picker when the framework has one. A custom dropdown that doesn't support keyboard navigation. Custom toggle switches with non-standard behavior.
- **Non-standard icon meanings.** Using a gear icon for something other than settings. Using a bell for something other than notifications. Unusual icon choices that require learning.
- **Unexpected behaviors.** Single-click actions where double-click is expected (or vice versa). Right-click menus in a web app. Drag-and-drop without any indication it's available.
- **Novel navigation patterns.** Bottom navigation in a desktop app. Horizontal scrolling for vertical content. Swipe gestures without alternative click actions.

**Severity guide:**
- Custom component that lacks standard keyboard support → CRITICAL
- Novel pattern replacing a well-known standard → WARNING
- Unusual icon choice for common concept → SUGGESTION

---

### Miller's Law

**"The average person can only keep 7 (±2) items in working memory."**

Translation: When you present more than 5-9 items without structure, users start losing track.

**What to look for in code:**
- **Long unstructured lists.** Arrays rendered as flat lists with 10+ items and no grouping, headers, or pagination.
- **Tables with too many columns.** Tables rendering 8+ columns that are all visible simultaneously.
- **Multi-step instructions.** Help text or onboarding flows presenting 7+ steps at once.
- **Form fields without grouping.** Forms with more than 7 fields visible at once and no visual sections (fieldsets, cards, dividers).
- **Navigation menus with flat lists.** More than 7 items in a single nav group without sub-categories.

**Severity guide:**
- 10+ visible form fields with no grouping → WARNING
- 8+ table columns all visible → SUGGESTION
- Flat list of 15+ items with no structure → WARNING

---

### Law of Proximity (Gestalt)

**"Objects that are near each other are perceived as related."**

Translation: Spacing is meaning. Close items = related. Distant items = unrelated.

**What to look for in code:**
- **Submit button far from form fields.** Large gap between the last form field and the action buttons.
- **Error messages distant from their field.** Error text rendered at the top or bottom of the form, not inline with the field.
- **Labels not visually connected to inputs.** Large gaps between label and input, or labels that are closer to the previous field than their own.
- **Action buttons far from the content they act on.** "Edit" button in the header when the editable content is in the body. "Delete" at the page top for a list item at the bottom.
- **Equal spacing everywhere.** When the gap between related items is the same as the gap between unrelated sections, grouping is lost.

**Severity guide:**
- Error messages not inline with fields → WARNING
- Submit button separated from form by other content → WARNING
- Equal spacing destroying visual grouping → SUGGESTION

---

### Law of Common Region (Gestalt)

**"Elements sharing an area with a clearly defined boundary are perceived as grouped."**

Translation: Cards, borders, and background color changes create visual groups. Use them intentionally.

**What to look for in code:**
- **Related items without visual containment.** Form sections that should be grouped but have no card, border, or background to contain them.
- **Too many containers.** Nested cards, borders within borders — visual noise that undermines the grouping benefit.
- **Inconsistent containment.** Some sections in cards, others floating free, with no design reason for the difference.

---

### Peak-End Rule

**"People judge an experience largely based on how they felt at its most intense point and at its end."**

Translation: The hardest moment and the final moment disproportionately shape the user's memory of the entire experience.

**What to look for in code:**
- **Frustrating peaks.** The most complex form, the slowest loading page, the most confusing flow — these will define the user's memory. Is the hardest moment softened with guidance, progress indicators, or reassurance?
- **Anticlimactic endings.** Multi-step flows that end with a generic "Done" message. Long form submissions that show only a brief toast. Checkout processes that don't confirm what was purchased.
- **No completion state.** Flows that simply redirect to a list page after completion — no celebration, no summary, no "here's what you just did."

**Severity guide:**
- Multi-step flow with no completion summary → WARNING
- Complex operation with no progress indication → WARNING
- Generic success message after major effort → DELIGHT (opportunity)

---

### Aesthetic-Usability Effect

**"Users often perceive aesthetically pleasing design as design that's more usable."**

Translation: Beautiful interfaces get the benefit of the doubt. Ugly or broken-looking interfaces make users distrust the functionality — even if it works correctly.

**What to look for in code:**
- **Inconsistent styling.** Mixed spacing, misaligned elements, inconsistent border radii — visual disorder signals unreliability.
- **Broken layouts.** Content overflowing containers. Text truncation without ellipsis. Images without aspect-ratio constraints.
- **Unstyled states.** Default browser checkbox styles, unstyled form validation messages, flash of unstyled content.

**Severity guide:**
- Content overflow or layout breakage → WARNING
- Inconsistent visual patterns → SUGGESTION
- Unstyled browser defaults → SUGGESTION

---

### Doherty Threshold

**"Productivity soars when a computer and its users interact at a pace (<400ms) that ensures neither has to wait on the other."**

Translation: If the UI feels sluggish, users lose focus and flow. Perceived performance matters as much as actual performance.

**What to look for in code:**
- **No optimistic updates.** Every mutation waits for server response before updating UI.
- **No skeleton screens.** Content areas show nothing until data loads, then pop in.
- **No loading indicators.** Async operations with no spinner, progress bar, or skeleton.
- **No debouncing on search/filter.** Every keystroke triggers a re-render or API call.

**Severity guide:**
- No loading state for async operations → WARNING
- No skeleton/placeholder for content areas → SUGGESTION
- Missing debounce on search input → SUGGESTION

---

### Postel's Law

**"Be liberal in what you accept, and conservative in what you send."**

Translation: Inputs should accept various formats gracefully; outputs should be clean and consistent.

**What to look for in code:**
- **Rigid input validation.** Phone fields rejecting "(555) 123-4567" because they expect "5551234567". Date fields requiring exact format. Case-sensitive search.
- **No input formatting.** User types "5551234567" and it stays as an unformatted number instead of being displayed as "(555) 123-4567".

**Severity guide:**
- Input rejection of common alternative formats → WARNING
- No input formatting for phone/currency/date → SUGGESTION

---

### Von Restorff Effect (Isolation Effect)

**"When multiple similar objects are present, the one that differs from the rest is most likely to be remembered."**

Translation: Make important things visually distinctive. If everything looks the same, nothing stands out.

**What to look for in code:**
- **Primary actions with no visual distinction.** The main CTA has the same styling as all other buttons.
- **Destructive actions not visually distinct.** Delete buttons using the same variant as edit or view buttons.
- **Important status not highlighted.** Critical alerts styled the same as informational messages.

**Severity guide:**
- Destructive action not visually distinct → WARNING
- Primary CTA same prominence as secondary actions → SUGGESTION

---

## Cross-Cutting Review Checklist

When reviewing with a Laws of UX lens, check:

1. **Target sizes** — Are all interactive elements at least 44×44px effective area? (Fitts's)
2. **Choice count** — Are users presented with more than 5-7 options without structure? (Hick's + Miller's)
3. **Pattern familiarity** — Are standard patterns used where they exist? (Jakob's)
4. **Spatial grouping** — Does spacing communicate relationships correctly? (Proximity)
5. **Visual distinction** — Do important elements stand out from their surroundings? (Von Restorff)
6. **Feedback speed** — Are operations responsive, or do users wait with no indication? (Doherty)
7. **Input flexibility** — Do inputs accept reasonable format variations? (Postel's)
8. **Peak moments** — Is the hardest/most critical moment of the flow handled with care? (Peak-End)
9. **Visual polish** — Does the interface earn the user's trust through consistency? (Aesthetic-Usability)
