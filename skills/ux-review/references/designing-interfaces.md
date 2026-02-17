# Designing Interfaces — Jenifer Tidwell (O'Reilly)

A deep-dive reference for UX code review through the lens of Tidwell's comprehensive UI pattern library. Use this when running `/ux-review:deep patterns`.

---

## Core Thesis

Good interface design isn't about invention — it's about **pattern literacy**. Users have encountered hundreds of interfaces. They've internalized patterns for how navigation works, how forms behave, how lists are organized, and how actions are presented. The designer's job is to recognize which pattern fits the context and implement it well — not to reinvent from scratch.

Tidwell catalogs dozens of UI patterns organized by user need. For code review, the key question is: **"Is the right pattern used for this context, and is it implemented correctly?"**

---

## Pattern Categories for Code Review

### 1. Navigation Patterns

**What to review:** How users move through the application. Is the navigation model clear, consistent, and appropriate for the app's depth?

#### Hub and Spoke
Content radiates from a central hub (dashboard, home page). Users go out to detail pages and return.

**Code signals of problems:**
- Detail pages with no way back to the hub (missing breadcrumb, no back link)
- Hub page with too many spokes (8+ direct links with no grouping)
- Inconsistent return pattern (some pages have "Back," others don't)

#### Pyramid
Hierarchical navigation: category → subcategory → item. Standard for content-heavy apps.

**Code signals of problems:**
- Deep hierarchy (4+ levels) with no breadcrumbs
- Intermediate levels that are just pass-through (click → click → click to reach content)
- No "skip levels" shortcut (search, recent items, favorites)

#### Modal Panel
Content that appears in an overlay (modal, dialog, slide-over) for focused tasks.

**Code signals of problems:**
- Modals used for complex multi-step flows (modals should be for focused, short tasks)
- Modal within modal (stacking modals destroys spatial context)
- No way to dismiss (missing close button, escape handler, backdrop click)
- Modal content that should be a full page (too much scrolling within a modal)

#### Tabs
Content organized into parallel panels, one visible at a time.

**Code signals of problems:**
- More than 7 tabs (too many for horizontal scanning)
- Tab labels that are too long or unclear
- Tabs that change the URL inconsistently (some do, some don't)
- Content between tabs that isn't truly parallel (one tab is a form, another is a list, another is settings)

### 2. Layout Patterns

**What to review:** How content is spatially organized within a page or component.

#### Visual Framework
The consistent structural elements that persist across pages: header, sidebar, content area, footer.

**Code signals of problems:**
- Layout structure that changes between pages (sidebar present on some, absent on others, without good reason)
- Inconsistent content area width or padding
- Header height or content that varies between pages

#### Card Grid
Content organized as a grid of equal-weight cards, each containing a summary.

**Code signals of problems:**
- Cards with widely varying content heights (causes jagged grid)
- Cards with no clear primary information (user can't quickly scan the grid)
- Too many cards visible at once (no pagination, filtering, or progressive loading)
- Cards that look interactive but aren't (or vice versa)

#### Detail View
A single item displayed with all its attributes, usually reached from a list or grid.

**Code signals of problems:**
- All attributes presented as a flat list with no grouping (information architecture failure)
- No visual hierarchy among attributes (name, status, and creation date all look the same)
- Related items (child records) not accessible from the detail view
- No actions available on the detail view (user must navigate elsewhere to edit/delete)

### 3. Form Patterns

**What to review:** How data input is structured and validated.

#### Forgiving Format
Inputs that accept multiple formats and normalize internally.

**Code signals of problems:**
- Phone inputs that reject formats with dashes, spaces, or parentheses
- Date inputs that require a specific format string
- Search that requires exact matches (no fuzzy, no partial)
- Case-sensitive inputs where case shouldn't matter

#### Structured Format
Inputs that guide the user into the correct format with masks, pickers, or separate fields.

**Code signals of problems:**
- Date entry as a text field instead of a date picker
- Currency entry without automatic formatting
- Address entry as a single text field instead of structured (street/city/state/zip)
- Phone entry without formatting mask

#### Good Defaults
Pre-filled values that match the most common choice.

**Code signals of problems:**
- Date pickers defaulting to today when another date is more likely (e.g., end-of-quarter for tax filings)
- Country/state selectors with no default when the app serves a specific region
- Form fields left blank when the system has the data to pre-fill them
- Toggle/checkbox options where the most common choice isn't the default

#### Input Hints
Helper text, examples, or tooltips that guide the user without cluttering the interface.

**Code signals of problems:**
- Complex fields with no helper text
- Placeholder text used as the only guidance (disappears on focus)
- Error messages that don't explain the expected format
- No character counts on length-limited fields

#### Fill-in-the-Blanks
Inline editing within a sentence or paragraph structure, reducing the "form" feel.

**Use when:** The data naturally fits a sentence ("Ship [quantity] units to [address] by [date]").

**Don't use when:** The form has many independent fields.

### 4. Table & Data Collection Patterns

**What to review:** How collections of items are presented, navigated, filtered, and acted upon. Tables are the single most common UI pattern in internal tools — they're where users spend most of their time. Getting tables right disproportionately impacts the overall experience.

#### When to Use a Table vs. Alternatives

| Collection Size | Fields Per Item | Best Pattern |
|----------------|-----------------|-------------|
| 1-5 items | Any | Card list or detail rows |
| 5-15 items | 1-2 fields | Simple list |
| 5-15 items | 3+ fields | Table (no pagination needed) |
| 15-50 items | 3+ fields | Table + search/filter |
| 50-200 items | 3+ fields | Table + search/filter + pagination |
| 200+ items | 3+ fields | Table + server-side search/filter/pagination |

**Code signals of wrong pattern choice:**
- Card grids for 50+ items with 5+ comparable fields (table would be better for scanning)
- Tables for 3 items (a list or card layout would feel less sterile)
- Tables loading full datasets of 200+ records client-side with no pagination

#### Column Design

**How many columns is too many?**
- **5-7 columns:** Comfortable for most screens. Users can scan without horizontal scrolling.
- **8-10 columns:** Pushing limits. Consider column visibility toggles or hiding lower-priority columns by default.
- **10+ columns:** Almost certainly too many visible at once. Users can't track which row they're reading across that many columns (Miller's Law applies to column count too).

**Code signals of problems:**
- Tables rendering 8+ `<TableHead>` elements with no visibility controls
- Important data in rightmost columns that require horizontal scrolling to see
- All columns at equal width — no prioritization of important vs. supporting data
- Column headers that are abbreviations or jargon without `title` attributes or tooltips
- Text truncation in columns without `title` or tooltip to reveal full content

**Column width decisions:**
- Primary identifier columns (name, ID, number) should be wider — they're the scan anchor
- Status columns need only enough width for the badge
- Date columns need consistent width (don't let them wrap)
- Action columns should be right-aligned and fixed width
- Resizable columns need a visual affordance — users won't discover resize on hover alone

#### Sorting

Sorting is expected behavior on any table with 10+ rows. Missing sort is a usability gap.

**Code signals of problems:**
- No sort indicators (which column is sorted? ascending or descending?)
- Sort icons on column headers but some columns aren't actually sortable (misleading affordance)
- Sort that resets when navigating away and back
- Inconsistent sort behavior: some tables sort client-side, others don't sort at all
- Default sort that doesn't match user expectations (alphabetical when chronological makes more sense, or vice versa)
- Sort icons that are hard to see (`h-3 w-3` sort indicators in dense headers)

**What good looks like:**
- Clear active sort indicator (arrow direction + visual emphasis on the sorted column)
- Sensible default sort (newest first for dates, alphabetical for names)
- Sort state persisted across page navigation
- Consistent sort capability across all table pages

#### Filtering & Search

For tables with 15+ rows, search and filtering aren't optional — they're how users actually find things. Users don't scroll through lists; they narrow them.

**Code signals of problems:**
- Tables with 20+ rows and no search input
- Search that requires exact matches (no fuzzy, no partial, no case-insensitive)
- Filters hidden in popovers or collapsed panels with no visual indicator that filters are active
- Active filters with no way to see which filters are applied at a glance
- No way to clear all filters with one action
- Filter options that are in random order (should be alphabetical or by frequency)
- Filters that cause a full page reload instead of client-side narrowing

**Filter discoverability patterns:**
- **Best:** Filter controls visible above the table (search bar + dropdowns inline)
- **Good:** Filter bar with active filter badges showing applied filters
- **Problematic:** Filters inside popovers with no badge count indicating active filters
- **Bad:** Filters on a separate page or in a modal

**What to check:**
- Are active filters visible without opening a popover? (filter badges, chips)
- Is there a "Clear all filters" option?
- Does the empty state distinguish "no items exist" from "no items match your filters"?
- Does the search input have a placeholder showing what fields it searches?

#### Pagination & Data Loading

Every table needs a strategy for when data grows beyond one screenful.

**Code signals of problems:**
- Tables loading entire datasets client-side with no pagination (performance degrades silently as data grows)
- Pagination with no indication of total count or current position ("Page 2" but of how many?)
- Neither pagination nor infinite scroll on tables with 50+ rows
- Loading states that show only a spinner with no context ("Loading..." — loading what?)
- No skeleton screens for table rows during loading

**Choosing a pagination strategy:**

| Strategy | Best For | Avoid When |
|----------|----------|------------|
| Client-side (load all, paginate locally) | < 200 items, simple data | Data will grow unbounded |
| Server-side pagination | 200+ items, complex filtering | Simple static lists |
| Infinite scroll | Feed-style chronological browsing | Users need to reach footer or specific position |
| "Load more" button | Moderate sets where user controls pace | Very large datasets |

**What good looks like:**
- Clear count: "Showing 1-25 of 347 work orders"
- Per-page size control for power users
- Persistent pagination state when navigating back to the table

#### Row Interaction

How users interact with individual table rows significantly impacts the experience.

**Code signals of problems:**
- Clickable rows on some tables but not similar others (inconsistent expectation — CON-04)
- Entire row is clickable but click target isn't clear (no cursor change, no hover state)
- Rows with `hover:bg-gray-50` but no `cursor-pointer` (suggests interactivity but isn't clickable)
- Clickable text links in cells where the whole row should be clickable (small targets)
- Click action is inconsistent: some rows open modals, similar rows on other pages navigate to pages

**Row click patterns:**
- **Click row → modal:** Good for quick views, editing. Keep the user in context.
- **Click row → detail page:** Good for complex records that need full-page space.
- **Click identifier → action, rest of row is not clickable:** Most explicit, but small click target.
- **Inconsistent across similar tables:** Bad. Pick one pattern and use it everywhere.

#### Action Columns

Every data table needs actions — view, edit, delete, etc. How these are presented matters.

**Code signals of problems:**
- Action buttons with different patterns across tables (sometimes icons, sometimes text, sometimes dropdowns)
- Destructive actions (delete) not visually distinct from view/edit actions
- Action column width that varies based on content (should be fixed)
- Too many actions visible (5+ icons per row) — consider an overflow menu
- Actions that disappear on hover only (users must discover them)
- Icon-only actions with no `aria-label` or `title` tooltip

**Action column best practices:**
- 1-3 common actions visible, remaining in a `...` overflow menu
- Destructive actions visually distinct (red icon, or separated by a divider)
- Consistent icon choices across all tables (pencil = edit everywhere, trash = delete everywhere)
- Right-aligned, fixed width

#### Empty States

Empty tables are the first-time user's experience and a regular occurrence when filtering.

**Code signals of problems:**
- No empty state — table just shows headers with no rows and no message
- Generic "No items found" with no distinction between "none exist" and "none match your filter"
- Empty state that doesn't offer a way to create the first item
- No icon or visual element to soften the blank space
- Empty state with same vertical height as a full table (massive blank space)

**Two empty states every table needs:**

1. **No items exist yet:** Encouraging, with a create action. "No work orders yet. Create one to get started."
2. **No items match filters:** Helpful, with a clear-filters action. "No work orders match your filters. Try adjusting your search or clearing filters."

#### Table Consistency Across Pages

In apps with many table pages, cross-table consistency is critical. Users build expectations from one table and apply them to all others.

**Cross-table review checklist:**
- Do all tables use the same component library (shadcn `<Table>` vs. raw `<table>`)?
- Are column headers styled identically across tables?
- Is row hover behavior consistent?
- Is the action column in the same position (rightmost) on all tables?
- Are status badges using the same color scheme across tables?
- Is sort behavior consistent (same icons, same interaction)?
- Are filters positioned in the same location relative to the table?
- Do all tables have empty states?
- Is date formatting consistent across all tables?
- Is the loading state treatment the same?

#### Mobile Table Behavior

Tables on narrow screens need explicit handling — they don't gracefully degrade on their own.

**Code signals of problems:**
- Tables wrapped in `overflow-x-auto` as the only mobile strategy (requires horizontal scrolling)
- No responsive card view alternative for narrow screens
- Important data in rightmost columns invisible without scrolling on mobile
- Touch targets (action buttons) too small in table cells on mobile

**Patterns for mobile tables:**
- **Responsive card transformation:** On small screens, each row becomes a card with stacked fields
- **Priority columns:** Hide non-essential columns on small screens, show only the 2-3 most important
- **Frozen first column:** Keep the identifier column visible while other columns scroll

#### Cascading Lists (Master-Detail)

Master → detail pattern where selecting an item in a list shows its details alongside.

**Code signals of problems:**
- List selection doesn't visually indicate which item is selected
- Detail panel doesn't update immediately (requires a page reload)
- No empty state in the detail panel when nothing is selected
- Detail panel has no close/collapse option
- Master list can't be resized relative to detail panel

### 5. Action Patterns

**What to review:** How the user triggers operations and receives feedback.

#### Button Groups
Related actions presented together with clear visual hierarchy.

**Code signals of problems:**
- All buttons the same visual weight (no primary/secondary distinction)
- Destructive actions not visually distinct from constructive actions
- Button group with 5+ buttons (too many choices — see Hick's Law)
- Button labels that are verbs without objects ("Save" not "Save Changes"; "Delete" not "Delete Work Order")

#### Confirmation
Asking the user to verify before executing a significant action.

**Code signals of problems:**
- Destructive actions with no confirmation dialog
- Confirmation dialogs with vague text ("Are you sure?")
- Confirmation that doesn't explain consequences ("Delete this item?" vs. "Delete this work order? This will also remove 3 associated documents.")
- Too many confirmations (every action asks "Are you sure?" — confirmation fatigue)

#### Progress Indicator
Visual feedback during multi-step processes or long operations.

**Code signals of problems:**
- Multi-step flows with no step indicator (where am I? how many steps?)
- Long operations with no progress bar or spinner
- Progress indicators that don't show completion (spinner only, no percentage or step count)
- Steps that can't be revisited (no back button in a multi-step flow)

#### Undo
Allowing users to reverse an action instead of (or in addition to) confirming before acting.

**Code signals of problems:**
- Destructive actions with confirmation but no undo (belt AND suspenders would be better)
- Actions that produce toasts with no "Undo" link
- Batch operations with no rollback mechanism

### 6. Information Display Patterns

#### Status Indicators
Visual markers showing the current state of an item.

**Code signals of problems:**
- Status shown only by color (no text label — accessibility issue ACC-02)
- Inconsistent status colors (green means "active" in one place, "completed" in another)
- Status values with no visual distinction (all rendered as plain text)
- Missing status on items that clearly have a state lifecycle

#### Notification
Messages that inform the user about events, changes, or results.

**Code signals of problems:**
- Success and error notifications using the same visual treatment
- Notifications that auto-dismiss too quickly (user can't read them)
- Notifications that stack and overlap
- No notification at all for completed operations

#### Empty State
What the user sees when a list, table, or content area has no items.

**Code signals of problems:**
- Raw "No data" or "0 results" text with no visual treatment
- Empty states that don't guide the user on what to do next
- Missing empty states entirely (blank white space with no explanation)
- Empty states that blame the user ("You haven't created any...")

---

## Pattern Mismatch Review Checklist

The most common UX issue Tidwell addresses is **using the wrong pattern for the context**. Check:

| Context | Right Pattern | Wrong Pattern |
|---------|--------------|---------------|
| Editing a single record | Modal or inline edit | Navigating to a new page |
| Complex multi-field entry | Full page form with sections | Modal with scrolling form |
| Browsing 100+ items | Table with sort/filter/search | Card grid with no filtering |
| Choosing from 3-5 options | Radio buttons or segmented control | Dropdown |
| Choosing from 10+ options | Searchable dropdown or combobox | Radio buttons |
| Showing item details from a list | Slide-over or master-detail | Navigating away from list |
| Destructive action | Confirmation with consequences explained | Immediate execution |
| Showing state of an item | Badge or status indicator with label | Color alone |
| Multi-step process | Stepper/wizard with progress | Long single-page form |

---

## Component-Level Pattern Review

### For Every Page
1. What navigation pattern is used? Is it appropriate for the app's depth?
2. Is the layout pattern consistent with other similar pages?
3. Are actions organized with clear primary/secondary hierarchy?

### For Every Form
1. Are the right input types used for each field's data type?
2. Are defaults sensible and helpful?
3. Is validation forgiving of format variations?
4. Are hints provided for non-obvious fields?

### For Every List/Table
1. Is the right collection pattern used (table vs. cards vs. list)?
2. Is there search/filter/sort for 15+ items?
3. Is there pagination or lazy loading for 50+ items?
4. Is there an empty state?

### For Every Action
1. Is there appropriate confirmation for destructive/irreversible actions?
2. Is there visual feedback after the action completes?
3. Is the action label specific (verb + object, not just verb)?
