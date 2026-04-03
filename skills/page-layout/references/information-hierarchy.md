# Information Hierarchy — Page Layout Reference

A reference for page layout decisions grounded in information hierarchy. Use this when designing or reviewing how content is organized, weighted, and prioritized on a page. The central question: **does the visual weight of each element match its actual importance to the user?**

---

## Core Principle

Every page has a purpose. The user arrived with a task or question. The page layout should answer that question or enable that task with minimal effort. Information hierarchy is the tool that makes this happen — it controls what the user sees first, what they see second, and what they have to go looking for.

A well-structured hierarchy means the user's eyes land on the right thing without conscious effort. A broken hierarchy means the user has to scan, hunt, and think — and every second of that erodes confidence and speed.

---

## Visual Hierarchy Principles

### Size and Weight Signal Importance

Treat size and typographic weight as the primary tools for communicating rank. The most important element on the page should be the largest or heaviest. The least important should be the smallest and lightest.

Apply this principle consistently:

- Make the page title or record identifier the most visually dominant text element. Use the largest font size and strongest weight available in the design system.
- Render primary status indicators, key metrics, or summary values at a size clearly larger than surrounding body text. A status badge at `text-xs` next to a heading at `text-2xl` sends the right signal. A status badge at `text-lg` next to a heading at `text-lg` sends no signal at all.
- Size action buttons proportionally to their importance. The primary action (Create, Save, Submit) should be visually larger or more prominent than secondary actions (Cancel, Reset, Export). Never render the primary action smaller than a secondary action.
- Apply font weight deliberately. Reserve `font-bold` or `font-semibold` for elements that deserve emphasis. When everything is bold, nothing is bold.

**Anti-pattern:** A dashboard where every card, metric, and label uses the same text size and weight. The user's eye has nowhere to land — everything competes equally and nothing wins.

**Heuristic mapping:** VIS-01 (size/weight signal importance).

### Position Signals Priority

In left-to-right languages, the top-left corner of the content area is the highest-priority position. Attention flows from top-left downward and to the right. Place the most important content where attention naturally starts.

Positional hierarchy rules:

- Place the record name, page title, or primary identifier at the top of the content area. Never bury the page identity below secondary metadata or breadcrumbs.
- Position primary actions (the thing the user most likely came to do) in the upper portion of the page, within the first visible viewport. A "Create Invoice" button below three paragraphs of explanatory text is a buried action.
- Place supplementary context (timestamps, audit info, secondary metadata) lower on the page or in secondary columns. This information supports the primary content but should never compete with it.
- In two-column layouts, place the primary content in the wider column. Use the narrower column for supporting metadata, related links, or secondary actions.

**Anti-pattern:** A detail page where the record's unique identifier and status are in a sidebar, while the main content area opens with a long description field. The user cannot immediately confirm they are looking at the right record.

**Heuristic mapping:** VIS-06 (content follows natural scan patterns).

### Color and Contrast Draw Attention

High contrast elements attract attention before low contrast elements. Use contrast intentionally to direct the eye toward what matters.

Contrast application guidelines:

- Reserve the highest-contrast color treatments (filled primary-color buttons, bold colored badges, high-saturation alerts) for critical information and primary actions.
- Render secondary and tertiary elements with reduced contrast — muted text colors, outlined buttons, subtle borders.
- Use semantic color consistently: red/destructive for warnings and deletes, green/success for confirmations, blue/primary for main actions. Do not use semantic colors decoratively — a green header on a non-success page trains users to ignore green.
- Limit the number of high-contrast elements per viewport to two or three. When five elements all scream for attention with bold colors and strong backgrounds, the hierarchy collapses and the user must read everything sequentially.

**Anti-pattern:** A form page where the Cancel button is a red filled button (`bg-red-500 text-white`) and the Submit button is a plain outlined button. The destructive action is visually dominant; the primary action is visually recessive. Contrast is working against the hierarchy.

**Heuristic mapping:** VIS-04 (contrast guides the eye to key elements).

### Whitespace Creates Breathing Room and Groups Related Items

Whitespace is not empty space — it is a structural element. It separates unrelated content, groups related content, and gives high-importance elements room to command attention.

Whitespace rules:

- Add more spacing between unrelated sections than between related items within a section. If the gap between two form field groups equals the gap between fields within a group, the grouping is invisible.
- Give primary elements generous surrounding whitespace. A key metric surrounded by padding reads as important. The same metric crammed between two other elements reads as just another value.
- Use whitespace to create visual lanes. A page with consistent vertical rhythm (heading, content, spacing, heading, content, spacing) scans faster than a page where spacing varies randomly.
- Never let content touch the edges of its container. Padding inside cards, panels, and page boundaries prevents the cramped feeling that makes users scan more slowly.

**Anti-pattern:** A settings page where every option is spaced identically — `mb-2` between every element, no section breaks, no group headers. The page is a flat list of 30 controls with no structure. The user must read every label to find the one they need.

**Heuristic mapping:** VIS-02 (whitespace gives elements room to breathe), VIS-05 (proximity groups related items).

### Typography Hierarchy

Establish a clear typographic scale and apply it with discipline. The scale should have at least four distinct levels with visible differentiation between each.

Typography levels:

- **Level 1 — Page title.** The largest, heaviest text on the page. One per page. Identifies what this page is or what record the user is looking at.
- **Level 2 — Section headings.** Clearly smaller than the page title but clearly larger than body text. Used to divide the page into scannable sections.
- **Level 3 — Body text and labels.** The default reading size. Comfortable for paragraphs, form labels, table cells, and descriptions.
- **Level 4 — Captions and metadata.** Smaller and lighter than body text. Used for timestamps, helper text, attribution, and secondary metadata that supports but does not compete with body content.

Each level must be visually distinct from the adjacent levels. If the user cannot immediately tell the difference between a section heading and a body label, the hierarchy has failed. Use a minimum ratio of 1.2x between adjacent levels (e.g., 14px body, 17px section heading, 20px page title).

**Anti-pattern:** A page using `text-sm` for everything — headings, labels, values, and captions. The page is technically readable but structurally flat. There is no visual shortcut to scan for sections or find the page identity.

**Heuristic mapping:** VIS-01 (size/weight signal importance), COG-02 (chunk information into digestible groups).

---

## Information Ranking Framework

Classify every piece of information on a page into one of three tiers before deciding on placement, size, or emphasis. This classification drives all layout decisions.

### Tier 1: Critical

Information that must be visible immediately, without scrolling, the moment the page loads. This is the reason the user came to this page. If the user glances at the page for two seconds and looks away, they should have absorbed this tier.

**Characteristics:**
- Largest text, highest contrast, highest position on the page.
- Occupies the primary content area (top-center or top-left of the main column).
- Never placed in a sidebar, tab, collapsible section, or below the fold.

**Examples by page type:**

| Page type | Critical information |
|-----------|---------------------|
| Record detail | Record name/ID, primary status, key metric |
| Dashboard | Top-level KPIs, alert count, primary navigation to action |
| List/index | Column headers, primary identifier of each row, filter controls |
| Form | Form title (what is being created/edited), primary fields, submit action |
| Settings | Currently active setting values, save action |

**Decision test:** If the user could only see one piece of information on this page before it disappeared, what would it be? That element is critical.

### Tier 2: Important

Information that the user needs for context or secondary tasks. Visible without scrolling ideally, but placed below or beside critical information — never competing with it. The user reads this tier after confirming the critical information answers their primary question.

**Characteristics:**
- Medium text size, moderate contrast, secondary position.
- Can occupy a sidebar, a secondary content section, or a row below the critical tier.
- May include secondary action buttons (Edit, Export, Share).

**Examples by page type:**

| Page type | Important information |
|-----------|----------------------|
| Record detail | Related metadata (dates, owner, category), secondary status fields, summary stats, edit action |
| Dashboard | Trend indicators, secondary metrics, recent activity summary |
| List/index | Row metadata (dates, status), secondary columns, bulk actions |
| Form | Non-primary fields, field-level help text, validation feedback |
| Settings | Setting descriptions, last-modified timestamps |

**Decision test:** If the user needs this information to understand or act on the critical information, it is important. If they can act on the critical information without it, re-evaluate whether it belongs in this tier.

### Tier 3: Nice-to-Have

Information that some users need occasionally but most users do not need on most visits. Accessible but not prominent. Place it where a user who specifically wants it can find it, but where it does not consume visual real estate that critical or important content needs.

**Characteristics:**
- Smallest text size, lowest contrast, lowest position.
- Appropriate placements: below the fold, inside collapsible sections, in secondary tabs, behind "Show more" toggles, in detail drawers.
- Never competes with Tier 1 or Tier 2 for space above the fold.

**Examples by page type:**

| Page type | Nice-to-have information |
|-----------|--------------------------|
| Record detail | Audit trail, change history, internal notes, system IDs, raw data |
| Dashboard | Historical comparison data, detailed breakdowns, export history |
| List/index | Rarely used columns, creation dates, internal IDs |
| Form | Advanced options, optional fields, help documentation links |
| Settings | Experimental features, developer options, API keys |

**Decision test:** Would removing this information from the visible page hurt most users' ability to complete their primary task? If not, it is nice-to-have.

### The Invisible Tier: Don't Show It

Not every piece of available data belongs on the page. Before placing information, ask: does any user need this on this specific page? Data that is available in the system is not automatically relevant to the current context.

Examples of information commonly shown but rarely needed:
- Database IDs displayed prominently when users think in terms of names or codes.
- Creation timestamps on active records where only the last-modified date matters.
- System-internal status codes alongside human-readable status labels.
- Admin-only metadata shown to non-admin users.

Remove these entirely or relegate them to a developer/debug mode. Every unnecessary element on the page weakens the hierarchy by adding noise that critical and important information must compete against.

---

## Mapping to UX Heuristics

Information hierarchy is not a standalone concern — it intersects with nearly every heuristic category in the ux-review system. The following mappings show how hierarchy decisions connect to specific heuristics.

### Visual Hierarchy Heuristics (VIS)

| Heuristic | Hierarchy connection |
|-----------|---------------------|
| VIS-01: Size/weight signal importance | Direct expression of the tier system. Tier 1 content must be visually largest/heaviest. Tier 3 must be visually smallest/lightest. When a Tier 3 element is the same size as a Tier 1 element, flag it. |
| VIS-02: Whitespace gives elements room to breathe | Critical-tier elements need surrounding whitespace proportional to their importance. A key metric crammed into a dense grid loses its hierarchical position even if it has the right font size. |
| VIS-03: Grid alignment creates visual order | A consistent grid enables hierarchy by giving each tier a predictable zone. When the grid breaks, the eye cannot predict where to look for Tier 1 vs. Tier 2 content. |
| VIS-04: Contrast guides the eye to key elements | Contrast enforcement for hierarchy. Tier 1 elements get highest contrast. Tier 3 elements get lowest. When multiple elements share the same high contrast, the hierarchy is ambiguous. |
| VIS-05: Proximity groups related items | Group items within the same tier together. Separate tiers with clear whitespace or dividers. When a Tier 3 element is physically adjacent to a Tier 1 element with no separation, it inherits unearned prominence. |
| VIS-06: Content follows natural scan patterns | Place Tier 1 content where scan patterns start (top-left for F-pattern, top-center for Z-pattern). Place Tier 3 content where scan patterns end. Reversing this placement forces users to fight their natural reading flow. |

### Cognitive Load Heuristics (COG)

| Heuristic | Hierarchy connection |
|-----------|---------------------|
| COG-01: Reduce decisions per screen | A clear hierarchy reduces perceived decisions. When the primary action is visually dominant and secondary actions are visually subordinate, the user does not experience them as competing choices. Flat hierarchy increases perceived decision count. |
| COG-02: Chunk information into digestible groups | Tier classification is a chunking exercise. Group Tier 1 elements together in one visual block, Tier 2 in another, Tier 3 in another. The user processes each chunk as a unit rather than each element individually. |
| COG-03: Progressive disclosure — show only what's needed now | Tier 3 content is the primary candidate for progressive disclosure. Collapse it, tab it, or hide it behind an interaction. Showing all three tiers simultaneously at full prominence defeats progressive disclosure. |
| COG-04: Recognition over recall — make options visible | Tier 1 and Tier 2 information should be visible (recognition). Tier 3 information can be hidden but its access point should be visible (the tab label, the "More" button, the collapsible header). Never hide information completely with no visible indicator that it exists. |
| COG-05: Consistent placement across pages | Apply the same tier placements across similar page types. If the record name is top-left on one detail page, it should be top-left on all detail pages. Inconsistent placement forces the user to re-learn the hierarchy on each page. |
| COG-06: Minimize context switching | Keep all Tier 1 and Tier 2 information for a given task on the same screen. If completing the primary task requires navigating to a different page to see Tier 2 context, the hierarchy is technically correct but the layout is functionally broken. |
| COG-07: Smart defaults reduce cognitive load | Apply to Tier 2 and Tier 3 form fields. Pre-fill predictable values so the user can focus attention on Tier 1 fields. A form where every field demands equal attention has no hierarchy. |

---

## Common Mistakes

### Flat Hierarchy — Everything Looks the Same

The most frequent hierarchy failure. Every element on the page uses the same text size, same weight, same color, same spacing. The user sees a wall of equally-weighted content and must read everything sequentially to find what they need.

**Detect by:** Scanning the page's CSS classes or design tokens. If fewer than three distinct text sizes are in use, the hierarchy is probably flat. If no element uses bold or semibold weight while others use normal, there is no weight-based differentiation.

**Fix:** Identify the single most important element. Make it visually dominant. Then identify the least important elements and make them visually recessive. The hierarchy will emerge from those two endpoints.

### Primary Action Buried or Hidden

The main thing the user came to do — create a record, submit a form, approve a request — is below the fold, inside a dropdown menu, or visually identical to secondary actions.

**Detect by:** Finding the primary action button and checking its position (is it above the fold?) and its visual weight (is it the most prominent interactive element?).

**Fix:** Move the primary action to the top of the page or pin it in a visible position. Use the highest-emphasis button variant (filled, primary color, largest size). Demote secondary actions to outlined or text-only button variants.

### Information Overload — Showing Everything at Once

Every piece of available data is rendered on the page simultaneously: all fields, all metadata, all history, all related records. The page is technically complete but practically unusable because the user cannot distinguish what matters from what does not.

**Detect by:** Counting the number of visible data points above the fold. If more than ten to twelve distinct pieces of information are visible simultaneously, the page is likely overloaded.

**Fix:** Apply the tier framework. Move Tier 3 content into collapsible sections, secondary tabs, or detail drawers. Reduce the above-fold content to Tier 1 and Tier 2 only.

### Mismatched Prominence

Nice-to-have information occupies more visual space or stronger emphasis than critical information. A common pattern: a large audit trail section dominates the page while the record's primary status is a small badge in the corner.

**Detect by:** Comparing the physical dimensions (height, width, and visual weight) of each content block against its tier classification. Any Tier 3 block that is physically larger than a Tier 1 block is a mismatch.

**Fix:** Resize or restructure so that physical prominence matches informational importance. Collapse the oversized Tier 3 section. Expand or emphasize the undersized Tier 1 element.

### Ignoring Scan Patterns

Important information placed in low-attention zones — bottom-right for F-pattern pages, far-right columns for dense layouts. The user's natural scan pattern skips over the most important content.

**Detect by:** Mapping Tier 1 elements to their physical position on the page. If Tier 1 content is in the bottom half of the page or in a narrow right column, it is in a low-attention zone.

**Fix:** Relocate Tier 1 content to the top-left or top-center of the content area. Use the F-pattern for content-heavy pages (key info along the left edge and top of each section). Use the Z-pattern for landing pages and dashboards (key info at the four corners of a Z shape).

### Too Many Competing Visual Elements

Multiple elements use high-emphasis styling: filled buttons, bold colors, large icons, colored backgrounds. The page feels loud, and no single element wins the user's attention.

**Detect by:** Counting the number of high-contrast, high-emphasis elements visible in a single viewport. If more than three elements use the highest-emphasis treatment, the visual hierarchy is competing rather than cascading.

**Fix:** Demote all but one or two elements. Pick the single most important element and give it the strongest treatment. Reduce everything else by at least one level of emphasis.

---

## Practical Decision Guide

Use this decision matrix when placing any piece of information on a page.

### Step 1: Classify the Information

| If the information... | Then it is... | Placement |
|-----------------------|---------------|-----------|
| Is needed to complete the user's primary task on this page | **Critical (Tier 1)** | Above the fold, highest visual emphasis, primary content area |
| Provides context for the primary task but is not required to act | **Important (Tier 2)** | Above fold if space allows, secondary emphasis, supporting position |
| Might be needed occasionally by some users | **Nice-to-Have (Tier 3)** | Below fold, collapsible sections, secondary tabs, detail drawers |
| Is not needed by any user on this specific page | **Remove it** | Do not show. Consider whether it belongs on a different page or not at all |

### Step 2: Validate Placement Against Visual Weight

After placing information, verify that the visual treatment matches the tier:

- **Tier 1 elements** use the largest text, strongest weight, highest contrast, and most prominent position. There should be no more than two to three Tier 1 elements per page.
- **Tier 2 elements** are clearly subordinate to Tier 1 — smaller text, lighter weight, or less prominent position. There can be several Tier 2 elements but they should be visually grouped, not scattered.
- **Tier 3 elements** are the smallest, lightest, and least prominent. They should be physically separated from Tier 1 and Tier 2 content by whitespace, dividers, or interactive boundaries (collapsed panels, tab boundaries).

### Step 3: Check for Hierarchy Violations

Scan the page for these specific failure modes:

1. **Tier inversion.** A Tier 3 element is more visually prominent than a Tier 1 element. This is always wrong — fix it immediately.
2. **Tier collapse.** Two adjacent tiers are visually indistinguishable. The user cannot tell critical from important or important from nice-to-have. Increase the contrast between tiers.
3. **Tier overcrowding.** More than three elements are classified as Tier 1. If everything is critical, nothing is critical. Re-evaluate and demote all but the true critical elements.
4. **Orphaned tier.** Tier 2 or Tier 3 elements are visually disconnected from the Tier 1 content they support. Group supporting information near the content it supports.

### Step 4: Test with the Two-Second Rule

Simulate a two-second glance at the page. In that time, the user should be able to answer:

- What page am I on? (Tier 1 — page title or record identifier)
- What is the state of the thing I care about? (Tier 1 — primary status or key metric)
- What can I do here? (Tier 1 — primary action)

If any of these questions require more than two seconds or any scrolling to answer, the hierarchy needs restructuring.
