# ux-review

A Claude Code plugin that reviews UI component and template code against UX principles from 6 research-backed books. Produces prioritized issue lists with file:line references and fix suggestions.

## Books

| # | Title | Author | Focus |
|---|-------|--------|-------|
| 1 | Don't Make Me Think | Steve Krug | Self-evident design, navigation, visual hierarchy |
| 2 | Rocket Surgery Made Easy | Steve Krug | Common usability problems, quick fixes |
| 3 | Laws of UX | Jon Yablonski | Fitts's, Hick's, Jakob's, Miller's Law |
| 4 | Design for How People Think | John Whalen | Cognitive psychology, mental models |
| 5 | Designing Interfaces | Jenifer Tidwell | UI patterns for layout, nav, forms, actions |
| 6 | Designing for Emotion | Aarron Walter | Personality, delight, emotional engagement |

## Commands

### `/ux-review:review`

Main review against all 48 heuristics across 8 categories.

```
/ux-review:review                      # Review common UI paths
/ux-review:review <path>               # Review specific file or directory
/ux-review:review --changed            # Review uncommitted changes
/ux-review:review --staged             # Review staged changes only
/ux-review:review --category=forms     # Focus on one category
```

Categories: `nav`, `cognitive`, `visual`, `forms`, `feedback`, `consistency`, `accessibility`, `delight`

### `/ux-review:deep`

Deep dive using a specific book's full reference file.

```
/ux-review:deep krug <path>            # Don't Make Me Think lens
/ux-review:deep rocket <path>          # Rocket Surgery lens
/ux-review:deep laws <path>            # Laws of UX lens
/ux-review:deep cognitive <path>       # Design for How People Think lens
/ux-review:deep patterns <path>        # Designing Interfaces lens
/ux-review:deep emotion <path>         # Designing for Emotion lens
/ux-review:deep forms <path>           # Cross-book: forms
/ux-review:deep navigation <path>      # Cross-book: navigation
/ux-review:deep tables <path>          # Cross-book: tables
```

## Agent

The `ux-reviewer` agent can be delegated to by other agents (code-reviewer, interface-design, etc.). It's read-only — reports findings, never modifies code.

## Heuristic Categories

| Category | ID Prefix | Count | Focus |
|----------|-----------|-------|-------|
| Navigation & Information Architecture | NAV | 7 | Wayfinding, navigation clarity |
| Cognitive Load & Complexity | COG | 7 | Decision overload, chunking, defaults |
| Visual Hierarchy & Layout | VIS | 6 | Prominence, whitespace, alignment |
| Forms & Input | FRM | 7 | Labels, validation, input types |
| Feedback & Error Handling | FBK | 5 | Loading, errors, confirmations |
| Consistency & Patterns | CON | 5 | Cross-page consistency |
| Accessibility & Inclusivity | ACC | 5 | Keyboard, color, targets |
| Delight & Personality | DLT | 6 | Celebrations, microcopy, empathy |

## Severity Levels

- **CRITICAL** — Actively harms usability, causes data loss, or blocks tasks
- **WARNING** — Degrades experience but doesn't block task completion
- **SUGGESTION** — Improvements that would elevate the experience
- **DELIGHT** — Opportunities to add personality and emotional engagement

## Design System Integration

When `.interface-design/system.md` exists in the project, the review includes bidirectional checks:

- **DS-CODE**: Code that violates the design system (e.g., using `text-base` when system says "Never text-base")
- **DS-UX**: Design system rules that conflict with UX principles (proposes specific edits to system.md)

## Installation

```bash
claude marketplace add BenVanZee/ux-review
claude plugin install ux-review@ux-review --scope user
```
