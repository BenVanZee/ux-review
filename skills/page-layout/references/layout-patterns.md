# Page Layout Patterns — Reference Guide

A practical reference for selecting and applying page layout patterns in business applications. Load this when advising on page structure, information placement, and action positioning for CRM systems, admin panels, portals, and internal tools.

---

## Core Principle

Every page has a job. The layout pattern exists to help the page do that job as clearly as possible. Select the pattern based on what the user needs to accomplish — not on what looks interesting or what the previous page used. A well-chosen layout makes the page feel obvious. A mismatched layout creates friction the user can feel but cannot name.

---

## Pattern 1: Detail Page (Record View)

The workhorse of business applications. Any time the user needs to view, understand, and act on a single record — a carrier, an invoice, a work order, a contact — reach for the detail page.

### When to Use

- Displaying one record with its full context: attributes, status, history, and related data.
- The user arrived here from a list or search and needs to understand this specific item.
- Actions need to be taken on this record (edit, approve, delete, escalate).

### Information Placement

**Header zone (top 80px).** Place the record's identity and state here. This means the record name or title, its current status (displayed as a badge or colored indicator), and the record type if not obvious from context. The header answers the question "What am I looking at?" within one second. Include a breadcrumb trail above or within the header so the user knows the path back.

**Key details section (immediately below header).** Arrange the record's core metadata as a grid of label-value pairs — typically two to four columns on desktop. Place the fields users check most frequently in the top-left positions (reading gravity pulls the eye there first). Limit this section to 8-12 fields. If the record has 30 attributes, group them and push secondary attributes into collapsible sections or tabs below.

**Related lists (below key details).** Display associated records — line items, activity history, related contacts, attached documents — as tables or card groups. Order related lists by frequency of use, not alphabetical order. The list the user checks most often goes first. Give each related list a clear heading and a count indicator ("Attachments (3)") so the user can scan without opening each section.

**Supporting context (bottom of page).** Place audit trails, system metadata, internal notes, and changelog entries here. This is information that matters occasionally but should never compete with operational data above.

### Sidebar Variant

For records with dense metadata, use a two-column layout: a narrow left sidebar (280-320px) containing key details as a vertical stack of label-value pairs, and a wide main content area containing related lists, activity feeds, and tabbed sections. This variant works well when the user needs to reference key details while scrolling through related data — the sidebar stays visible.

### Action Placement

**Primary action in the header.** Place the single most important action — "Edit," "Approve," "Process" — as a prominent button in the top-right of the header. This action should be visible without scrolling on any screen size.

**Secondary actions in a dropdown or button group.** Actions like "Duplicate," "Export," "Transfer" belong in a "More actions" dropdown or a secondary button group next to the primary action. Limit visible secondary actions to two or three; move the rest into the dropdown.

**Destructive actions isolated and protected.** Place "Delete," "Void," or "Archive" inside the dropdown menu, visually separated from other items (a divider above), styled in red or with a warning icon. Never place destructive actions next to the primary action at the same visual weight.

**Inline actions on related lists.** Each row in a related list may have its own actions (edit, remove, view). Display these as icon buttons on hover or as a small action menu at the end of each row. Keep them visually quiet so they do not compete with the page-level actions.

### Responsive Behavior

On tablet (768-1024px), collapse the key details grid from four columns to two. Maintain the header and primary action button at full width.

On mobile (below 768px), stack everything into a single column. The sidebar variant collapses — sidebar content moves above the main content area, or tucks behind a toggle. Related lists switch from tables to stacked cards. The primary action button becomes sticky at the bottom of the viewport so it remains accessible regardless of scroll position.

### Example Use Cases

- Carrier detail page in a compliance system: header shows carrier name + DOT number + active/suspended status; key details show MC number, physical address, contact info, insurance expiration; related lists show vehicles, drivers, open work orders, filing history.
- Invoice detail in a factoring app: header shows invoice number + status (pending/funded/paid); key details show debtor name, amount, due date, advance rate; related lists show payment history, attached documents, notes.
- Customer profile in a CRM: header shows company name + account tier; key details show contacts, industry, revenue band, assigned rep; related lists show opportunities, support tickets, recent activity.

---

## Pattern 2: Dashboard

The overview page. Dashboards aggregate data from across the system into a single at-a-glance view. They answer "How are things going?" and "What needs my attention?"

### When to Use

- The user needs a high-level summary of system state, performance, or activity.
- Multiple data sources need to be synthesized into one view.
- The page serves as a launchpad for diving into specific areas.

### Information Placement

**Metric cards row (top of page).** Display 3-6 key performance indicators as large-number cards in a horizontal row. Each card shows a label, a primary metric value, and a trend indicator (up/down arrow, percentage change, sparkline). Place the most business-critical metric in the leftmost position.

Resist the urge to add more metric cards. Every metric should answer the question: "If this number changed dramatically, would someone take action today?" If the answer is no, remove it. Dashboards become useless when they show everything — they become useful when they show only what matters.

**Alert or attention zone (below metrics).** If there are items requiring action — overdue tasks, failed processes, approaching deadlines — display them immediately below the metric cards as a compact warning banner or a short prioritized list. This zone answers "What needs my attention right now?" and should be absent entirely when nothing requires attention (do not show an empty section).

**Charts and trend visualizations (middle section).** Place 1-3 charts showing trends over time, distributions, or comparisons. Use a two-column grid for charts when displaying two. Never display more than four charts on a single dashboard — if more are needed, create separate analytics pages and link to them.

**Recent activity or item lists (below charts).** Display a feed of recent actions, newly created records, or updated items. Limit to 5-10 items with a "View all" link. This section bridges the dashboard to the operational detail — the user sees something interesting and clicks through.

**Quick action buttons.** Place 2-4 shortcut buttons for the most common tasks ("New Work Order," "Run Report," "Add Carrier") either in the header area or as a small action bar between the metrics and the charts. These are convenience shortcuts, not the primary purpose of the page.

### Action Placement

Dashboards are primarily informational, not transactional. Keep actions minimal. The main interactions are: clicking a metric card or chart to drill into details, clicking a recent item to open its detail page, and using quick action buttons to start common workflows. Avoid placing edit forms, inline data entry, or complex interactions directly on the dashboard.

### Responsive Behavior

On tablet, metric cards wrap to two rows if needed (3 per row instead of 6). Charts stack vertically in a single column. Activity feeds remain full-width.

On mobile, metric cards become a horizontal scrollable strip or stack vertically (showing the top 3-4 metrics, with others accessible via scroll or a "Show more" toggle). Charts stack vertically and simplify — consider swapping detailed charts for simplified sparklines or summary numbers. Activity feed remains a vertical list.

### Example Use Cases

- Operations dashboard for a trucking compliance firm: metric cards show active carriers, filings due this week, overdue filings, revenue MTD; alert zone shows carriers with expiring insurance; chart shows filing volume by month; recent activity shows latest completed work orders.
- Factoring dashboard: metric cards show total funded today, outstanding reserves, aging receivables, new applications; chart shows funding volume trend; recent activity shows latest invoices submitted.

---

## Pattern 3: List/Table View

The collection browser. Use this whenever the user needs to find, scan, filter, sort, or act on multiple records of the same type.

### When to Use

- Displaying a searchable collection of records (carriers, invoices, users, work orders).
- The user needs to compare records, find specific items, or perform bulk operations.
- The data is naturally tabular — multiple records with consistent attributes.

### Information Placement

**Search and filter bar (top, full width).** Place a search input prominently at the top-left. Adjacent to search, provide filter controls — dropdowns, date pickers, or a filter button that reveals a filter panel. Show active filters as removable chips/tags so the user always knows what subset they are viewing. Include a "Clear all filters" action when any filter is active.

**Results count and sort controls (between filters and table).** Display the total matching record count ("247 carriers" or "Showing 1-25 of 247"). Place sort controls here or integrate sorting into clickable column headers.

**Table (main content area).** Each row represents one record. Choose default visible columns carefully — show only the 4-7 columns the user needs for scanning and identification. Common defaults: name/identifier, status, key date, key metric, assigned owner. Provide a column visibility control (gear icon or "Columns" dropdown) for users who need different columns.

Make the primary identifier column (name, number, ID) a clickable link styled distinctly. This is the entry point to the detail page — it must be visually obvious.

**Empty state (when no results).** Display a clear message explaining why the table is empty. Distinguish between "no records exist yet" (show a create action and guidance) and "no records match your filters" (show a "Clear filters" action). Never show a blank white space with no explanation.

### Action Placement

**Page-level actions (top-right, aligned with search bar).** Place the primary creation action ("Add Carrier," "New Invoice") as a prominent button in the top-right corner. Secondary page-level actions (import, export) sit next to it or in a dropdown.

**Bulk actions toolbar.** When the user selects one or more rows via checkboxes, display a toolbar — either replacing the filter bar or appearing as a fixed bar at the top or bottom of the table. Show the selection count ("3 selected") and available bulk actions (delete, export, reassign, change status). Destructive bulk actions require confirmation.

**Row-level actions.** Place per-row actions in the last column as icon buttons or a three-dot menu. Keep row actions to 2-3 icons maximum visible; additional actions go in the overflow menu. Common row actions: edit, view, delete, duplicate.

### Key Decisions

Decide on column defaults before building. Ask: "If a user opens this list for the first time, which columns let them find what they need?" Default columns should enable identification and triage. Columns for deep analysis or edge cases should be available but hidden by default.

Choose between pagination and infinite scroll based on the use case. Pagination works better when users need to reference their position ("I was on page 3") or when total count matters. Infinite scroll works for feeds and casual browsing. For most business apps, prefer pagination.

### Responsive Behavior

On tablet, hide lower-priority columns and offer a "Columns" toggle. Maintain the search bar and primary action button.

On mobile, transform the table into a card list. Each card shows the record's key identifier, status, and one or two supporting details. Tapping a card navigates to the detail page. Filters move behind a "Filter" button that opens a slide-over panel. Bulk selection becomes a long-press or "Select mode" toggle.

### Example Use Cases

- Carrier list in a compliance system: columns show carrier name, DOT number, MC number, status, state, next filing due date; filters for status, state, filing type.
- Invoice list in factoring: columns show invoice number, debtor, amount, status, submitted date, funded date; filters for status, date range, debtor.
- User management in an admin panel: columns show name, email, role, status, last login; filters for role, status.

---

## Pattern 4: Form Page

Data entry and editing. Use this whenever the user needs to create or modify a record.

### When to Use

- Creating a new record (carrier, invoice, work order, user).
- Editing an existing record's attributes.
- Collecting structured input that will be validated and saved.

### Information Placement

**Single-column layout. Always.** Never arrange form fields side-by-side in multiple columns. Research consistently shows that single-column forms have higher completion rates and fewer errors. The eye tracks straight down, and the user never has to decide whether to read across or down. The only exception: closely related short fields that form a logical unit (city + state + zip on one row, first name + last name on one row).

**Logical field grouping.** Divide fields into sections with clear headings ("Company Information," "Contact Details," "Filing Preferences"). Group fields by topic, not by data type. Within each group, order fields from most important to least important, or follow the natural information flow (name before address, start date before end date).

**Labels above fields.** Place labels directly above their input fields, left-aligned. Never use placeholder text as the only label — placeholders disappear on focus and the user loses context. Use helper text below the field for format hints or constraints ("Must be 7 digits," "Format: MM/DD/YYYY").

**Required field indicators.** Mark required fields with an asterisk and include a legend at the top of the form ("* Required"). Alternatively, if most fields are required, mark the optional fields instead ("Optional") to reduce visual noise.

### Multi-Step Variant

For forms with more than 15-20 fields, or when fields fall into distinct logical phases, use a multi-step form (wizard).

**Step indicator (top of form).** Display a horizontal stepper showing all steps with labels: "1. Company Info," "2. Contact Details," "3. Filing Setup," "4. Review." Highlight the current step and mark completed steps. Allow clicking completed steps to navigate back.

**One section per step.** Each step contains one logical group of 4-8 fields. Show only the current step's fields. Include "Back" and "Next" buttons at the bottom. Validate the current step's fields on "Next" — do not allow advancing past invalid data.

**Review step.** Include a final review step that summarizes all entered data in a read-only format before submission. Allow the user to jump back to any section to make corrections.

### Action Placement

**Primary action at the bottom of the form.** Place the submit button ("Save Carrier," "Create Invoice," "Submit Filing") at the bottom of the form, left-aligned with the form fields. Label the button with a specific verb-noun phrase — never just "Submit" or "Save."

**Sticky primary action for long forms.** When the form is long enough to scroll (more than one viewport height), make the primary action button sticky at the bottom of the viewport, inside a bar that floats above the page content. This ensures the user can always see and reach the submit action.

**Cancel/discard action.** Place a "Cancel" or "Discard" link (not a button — lower visual weight) to the left of the primary action or as a text link. Always confirm before discarding if the user has entered data.

**Delete on edit forms.** When editing an existing record, place the delete action away from the save action — in the page header, in a "Danger zone" section at the very bottom, or in the header dropdown. Never place "Delete" adjacent to "Save."

### Smart Defaults and Pre-fill

Pre-populate fields wherever possible. If creating a record from a related context (new work order for a specific carrier), pre-fill the carrier field. Set sensible defaults for dropdowns and date fields (today's date, the user's default state, the most common option). Every pre-filled field is one less decision the user has to make.

### Validation

**Inline validation on blur.** Validate each field when the user moves to the next field, not only on form submission. Display error messages directly below the field in red text. Include a specific correction instruction ("Enter a valid email address"), not just "Invalid input."

**Form-level error summary.** On failed submission, display an error summary at the top of the form listing all invalid fields as clickable links that jump to the offending field. Scroll the user to the error summary automatically.

### Responsive Behavior

Single-column forms naturally adapt to mobile with minimal changes. Reduce field padding slightly and ensure tap targets meet the 44x44px minimum. The sticky submit bar remains at the bottom of the viewport. Multi-step forms work well on mobile since they already show a limited number of fields per screen.

### Example Use Cases

- New carrier registration form: sections for company info (name, DOT, MC, address), contact details (primary contact, phone, email), and filing preferences (IFTA jurisdiction, IRP base state, filing frequency).
- Invoice submission in factoring: fields for debtor selection, invoice number, amount, date, terms, with document upload for backup.
- Multi-step new work order: Step 1 selects the carrier and work order type; Step 2 collects the filing-specific details; Step 3 gathers documents; Step 4 reviews and submits.

---

## Pattern 5: Split View (Master-Detail)

A list and a detail view shown simultaneously. Use this for browse-and-inspect workflows where the user moves quickly between records.

### When to Use

- The user needs to browse a list and view details without losing their place in the list.
- Frequent back-and-forth between records (reviewing, triaging, comparing).
- The detail content is moderate in length — not so long that it competes with the list for space.

### Information Placement

**Left panel (master list, 280-360px wide).** Display a compact list of records. Each list item shows the minimum needed for identification: name or title, a status indicator, and one secondary detail (date, amount, assignee). Highlight the currently selected item with a distinct background color. Include a search input at the top of the list panel.

**Right panel (detail, remaining width).** Display the full detail of the selected record. Structure this panel like a condensed detail page: key information at the top, supporting details below. Include action buttons within the detail panel (not in the list panel).

**Divider.** Place a visible vertical divider between the panels. Optionally make it draggable for user-adjustable panel widths.

### Action Placement

Place actions in the detail panel's header or toolbar. The list panel is for navigation only — avoid placing edit or delete actions in list item rows (a "remove" icon on a list item is acceptable for lightweight removal, but major actions belong in the detail panel).

When no item is selected, display an empty state in the detail panel with guidance: "Select an item from the list to view details."

### Responsive Behavior

On tablet, narrow the list panel to 240px and give the remaining space to the detail panel.

On mobile, abandon the split view entirely. Show the list as a full-screen list view. Tapping a list item navigates to a full-screen detail view with a back button to return to the list. Do not attempt to show both panels on a narrow screen — it creates an unusable experience.

### Example Use Cases

- Email or messaging interface: message list on the left, full message on the right.
- Filing queue in a compliance system: list of pending filings on the left (carrier name, filing type, due date), full filing details and action buttons on the right.
- Document review: list of uploaded documents on the left, document preview and metadata on the right.

---

## Layout Anti-Patterns

Recognize and flag these common layout failures.

### The Junk Drawer Dashboard

A dashboard with 12+ metric cards, 6 charts, 3 tables, a calendar widget, and a news feed. No visual hierarchy — everything competes for attention and nothing wins. The user scans the page, absorbs nothing, and navigates away.

**Fix:** Ruthlessly prioritize. Limit to 4-6 metric cards, 2-3 charts, and one activity list. Move everything else to dedicated pages and link from the dashboard.

### The Mega-Form

A single scrolling page with 40-60 form fields and no section breaks, no grouping, no progressive disclosure. The user opens the page and immediately feels overwhelmed.

**Fix:** Group fields into logical sections with headings. If sections are distinct phases, use a multi-step form. Hide optional and advanced fields behind a "Show more" toggle or in a collapsible section.

### The Hidden Action

The primary thing the user needs to do on this page — the whole reason they navigated here — is buried in a three-dot menu, tucked inside a tab, or positioned below three screens of scrolling. The user hunts for it, cannot find it, and files a support ticket.

**Fix:** Place the primary action in the header, visible without scrolling, styled as the most prominent interactive element on the page.

### Symmetry Theater

A two-column or three-column layout where all columns are equal width, even though the content is not equal in importance. A 50/50 split between a critical data table and a decorative sidebar. A three-card layout where one card has 50 words and another has 5.

**Fix:** Size columns proportional to content importance. Use 2/3 + 1/3 or 3/4 + 1/4 splits. Give the primary content the dominant space.

### The Scroll Canyon

The user's primary task (an action button, a key data point, a form they need to fill out) sits below multiple decorative sections, welcome banners, or informational blocks. The user must scroll past nice-to-have content to reach need-to-have content.

**Fix:** Place critical content and actions above the fold. Push supporting and contextual content below. If a banner or intro section pushes operational content down, shorten or remove it.

### Tab Overload

Eight, ten, or twelve tabs across the top of a detail page, with vague labels like "Info," "Details," "More," "Other," "Advanced," "Settings," "Config." The user cannot predict which tab holds the information they need and resorts to clicking each one sequentially.

**Fix:** Limit tabs to 5-6 maximum. Use clear, specific labels ("Contact Info," "Filing History," "Documents" — not "Details" or "More"). Consolidate related content. Move rarely used sections to a linked sub-page rather than a tab.

---

## Choosing the Right Pattern

Use this decision guide to select the primary layout pattern.

| User's goal | Pattern |
|---|---|
| View one record with its full context and related data | Detail Page |
| Get a high-level overview of system state and key metrics | Dashboard |
| Find, browse, filter, or compare a collection of records | List/Table View |
| Create or edit a record by entering structured data | Form Page |
| Browse a list while inspecting individual items without leaving | Split View |

If the user's goal does not map cleanly to one pattern, identify the primary goal and use that pattern as the foundation. Layer secondary patterns into the layout as subordinate elements.

---

## Combining Patterns

Real-world pages frequently blend patterns. Maintain clarity by treating one pattern as the host and others as guests.

**Dashboard with embedded list views.** The dashboard is the host. Embed a compact table (5-10 rows, limited columns, no inline editing) as a dashboard widget. Include a "View all" link that navigates to the full list/table view. Do not try to replicate full search, filter, and bulk action functionality inside the embedded list.

**Detail page with inline editing.** The detail page is the host. Allow individual fields or sections to switch between read and edit mode on click. Display a small "Edit" icon or pencil next to editable sections. Save changes inline with auto-save or a per-section "Save" button. Do not convert the entire detail page into a form — that is a different pattern (navigate to a form page instead).

**List view with slide-out detail panel.** The list view is the host. Clicking a row opens a slide-over panel from the right (320-480px wide) showing a condensed detail view. This is a lighter-weight version of the split view pattern. The slide-out should provide enough detail for quick triage; include a "View full details" link for the complete detail page.

**Detail page with tabs containing list views.** The detail page is the host. Related data displays as tabbed lists below the key details section. Each tab contains a focused table for one type of related record. Include add/remove actions within each tab but avoid deep editing — link to the related record's own detail page for full interaction.

When combining patterns, ensure there is never ambiguity about which element is the page's primary content and which elements are supporting. Size, position, and visual weight should all reinforce the hierarchy. The host pattern occupies the dominant screen area. Guest patterns are visually contained — in cards, panels, or clearly bounded sections — so they do not blur into the host.
