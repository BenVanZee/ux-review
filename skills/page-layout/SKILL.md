---
name: page-layout
description: This skill should be used when the user asks to "design a page layout", "what should this page show", "create a page mockup", "restructure this page", "page information hierarchy", "what's important on this page", "plan a page", "lay out this screen", "what information goes on this page", or needs to identify key information, related lists, and visual hierarchy for a new or existing page. Produces structured page specs (.md) and interactive HTML mockups.
---

# Page Layout

Design page layouts by identifying what information matters, its relative importance, and how to arrange it. Produces two outputs: a structured page spec (`.md`) for AI agents and an interactive HTML mockup for visual review.

## When to Use

- Designing a new page or screen from scratch
- Restructuring an existing page that feels cluttered or unfocused
- Documenting what a page should show before building it
- Creating mockups for stakeholder review before coding

---

## Mode A: Design New Page

Guided interview to define page requirements, then generate outputs.

### Interview Process

Ask questions **one at a time**. Wait for each answer before asking the next.

1. **What is this page?** — Name, purpose, which app, who uses it.
2. **What's the primary goal?** — The #1 thing the user is trying to accomplish on this page. Everything else is secondary to this.
3. **What information must be visible immediately?** — The "above the fold" content. For each item, ask the user to rank: critical / important / nice-to-have.
4. **What related data should be accessible?** — Related lists, linked records, secondary info sections. Rank these too.
5. **What actions can be taken?** — Primary CTA, secondary actions, destructive actions. Ask where each should live on the page.
6. **What's the data volume?** — Is this a detail page for one record, a list of 10 items, or a searchable table of thousands? This determines the layout pattern.
7. **Sample data** — Ask the user to provide realistic example data (names, numbers, statuses, dates) or point to existing records in the app. Mockups must use realistic data, not "Lorem ipsum."
8. **Navigation context** — Where does the user come from? Where do they go next? What breadcrumb or back-link should appear?
9. **States** — What states does this page have? At minimum: populated, empty, error. Ask about loading states if the page fetches data.

### Interview Guidelines

- **One question at a time.** Do not batch questions.
- **Use the user's terminology.** If they say "schedule" not "invoice batch," use "schedule."
- **Probe for ranking.** If the user lists 10 things as "important," push back: "If you could only show 3 of these, which 3?"
- **Watch for scope creep.** If the page is trying to do too much, suggest splitting into multiple pages.

---

## Mode B: Restructure Existing Page

Analyze an existing page and propose improvements.

### Process

1. **Read the existing page code** — template files, component files, route definitions.
2. **Identify current information hierarchy** — What's prominent? What's buried? What's missing? Map to the critical / important / nice-to-have framework.
3. **Apply UX heuristics** — Consult `references/information-hierarchy.md` for principles. Key heuristics from the ux-review skill:
   - VIS-01 through VIS-06 (visual hierarchy)
   - COG-01 through COG-07 (cognitive load)
4. **Interview the user** — Ask what's wrong with the current page, what matters most, and what they wish was different.
5. **Propose restructured layout** — Present the information hierarchy changes with rationale, then generate outputs.

---

## Outputs

Both modes produce two files. Create the `docs/pages/` directory if it doesn't exist.

### 1. Page Spec — `docs/pages/<page-name>.md`

Structured documentation for AI agents to reference when building or modifying the page.

```markdown
# <Page Name>

**App:** <which application>
**Used by:** <role>
**Primary goal:** <what the user is trying to accomplish>
**Navigation:** <where user comes from → this page → where they go>

## Information Hierarchy

### Critical (always visible, prominent)
- <item> — <why it matters>

### Important (visible, secondary prominence)
- <item> — <why it matters>

### Nice-to-Have (accessible but not prominent)
- <item> — <why it matters>

## Related Data
- <Related list/section> — <what it shows, why it's here, approximate record count>

## Actions
- **Primary:** <main CTA — what it does, where it lives>
- **Secondary:** <other actions>
- **Destructive:** <delete/archive/etc — requires confirmation>

## Layout Pattern
<Which pattern from references/layout-patterns.md and why>

## Layout Notes
<UX rationale — why this arrangement serves the primary goal, references to heuristics>

## ASCII Layout Sketch
<Box-drawing layout showing spatial arrangement of sections>

## States
- **Populated:** <normal state with data>
- **Empty:** <what to show when no data — message, illustration, CTA>
- **Loading:** <skeleton/spinner approach>
- **Error:** <error handling approach>

## Sample Data
<Realistic example records provided by user>
```

### 2. HTML Mockup — `docs/pages/<page-name>-mockup.html`

Self-contained, interactive HTML file for visual review.

**Start from the template** at `assets/page-template.html`. Customize it by:
- Setting the page name in the demo nav bar
- Filling placeholder sections with content from the interview
- Using realistic sample data provided by the user
- Implementing each state (populated, empty, loading, error) as switchable screens
- Adjusting the color scheme if the project has established brand colors
- Applying the layout pattern identified during the interview

**Mockup requirements:**
- Single-file, no external dependencies
- Sticky demo nav bar for switching between states
- Responsive — should look reasonable at desktop, tablet, and mobile widths
- Visual hierarchy must reflect the information ranking from the spec
- Realistic data throughout — never placeholder text
- Professional styling appropriate to the app's domain

---

## Choosing a Layout Pattern

Consult `references/layout-patterns.md` for detailed guidance. Quick reference:

| User's answer to "data volume" | Pattern |
|-------------------------------|---------|
| One record with related data | **Detail Page** |
| Aggregated metrics + recent activity | **Dashboard** |
| Collection to browse/search | **List/Table View** |
| Entering or editing data | **Form Page** |
| Browsing list while viewing details | **Split View** |

---

## Additional Resources

### Reference Files

For detailed principles and patterns, consult:
- **`references/information-hierarchy.md`** — Visual hierarchy principles, information ranking framework, common mistakes, mapping to ux-review heuristics
- **`references/layout-patterns.md`** — Detailed layout patterns (detail page, dashboard, list view, form, split view), anti-patterns, pattern selection guide

### Assets

- **`assets/page-template.html`** — Base HTML template with demo nav bar, CSS utilities, component styles, and state switching. Use as starting point for all mockups.

---

## Integration with Other Skills

### Workflow docs (`docs/workflows/`)

If workflow docs exist (created by `/ux-review:build-workflows`), check them before designing a page. Workflows tell you what the user is trying to accomplish at each step — use that context to inform the information hierarchy.

### UX reviews (`/ux-review:review`)

Page specs in `docs/pages/` are loaded during UX reviews. The review checks if the built UI matches the documented information hierarchy and flags mismatches with PS-XX findings.

### Design system (`.interface-design/system.md`)

If a design system exists, the HTML mockup should use its color palette, typography, and spacing tokens rather than the template defaults.

---

## Project Setup

When using this skill in a project for the first time, suggest adding the following to the project's `CLAUDE.md`:

```markdown
## UX Design Workflow

When building new pages, features, or layouts:
- Before building a new page or significantly restructuring an existing one, suggest running the page-layout skill to define information hierarchy and generate a mockup first.
- Before building features with multi-step user flows, suggest running `/ux-review:build-workflows` to document the workflow first.
- Page specs live in `docs/pages/`. Workflow docs live in `docs/workflows/`.

When building or modifying ANY page:
- First check `docs/pages/` for an existing page spec. If one exists, use it as the source of truth for information hierarchy, layout, and what matters.
- Check `docs/workflows/` for workflows that touch this page. Use them for context on what the user is trying to accomplish and what data they need.
- If no spec or workflow exists yet, proceed but note the gap.
```

---

## Important

- **One page per session.** Keep it focused. If the user describes a multi-page flow, suggest documenting each page separately.
- **Realistic data is non-negotiable.** If the user hasn't provided sample data by question 7, do not proceed until they do. Mockups with placeholder data are useless for design review.
- **Rank ruthlessly.** Push back if everything is "critical." If a page has 15 critical items, it has zero — nothing stands out.
- **The spec is the source of truth.** The HTML mockup is a visual aid. The .md spec is what AI agents reference when building. Keep it accurate and up to date.
- **Create the directory** `docs/pages/` if it doesn't exist.
