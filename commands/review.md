---
name: ux-review:review
description: Review UI code against UX principles from 6 research-backed books. Produces prioritized issue lists with file:line references and fix suggestions.
---

# UX Review

Review UI code for usability, cognitive load, accessibility, and emotional design.

## Usage

```
/ux-review:review                      # Review common UI paths
/ux-review:review <path>               # Review specific file or directory
/ux-review:review --changed            # Review uncommitted changes
/ux-review:review --staged             # Review staged changes only
/ux-review:review --category=forms     # Focus on one category
```

**Category shortcuts:** `nav`, `cognitive`, `visual`, `forms`, `feedback`, `consistency`, `accessibility`, `delight`

## Process

Follow the review process defined in the `ux-review` skill exactly:

1. **Read the ux-review SKILL.md** — load all 48 heuristics and the review process
2. **Identify target files** from arguments, git diff, or common UI path globs
3. **Load design system** — read `.interface-design/system.md` if present in the project
4. **Review each file** — identify component type, apply relevant heuristic subset, look for code signals
5. **Cross-file consistency check** — compare patterns across reviewed files
6. **Design system bidirectional check** — DS-CODE (code violates system) + DS-UX (system conflicts with UX)
7. **Output the prioritized issue list** using the format from SKILL.md

## Important

- **Read the actual code** — this is a code review, not a screenshot review. Read every file before commenting on it.
- **Be specific** — every finding must include a file:line reference and a concrete fix suggestion.
- **Cite the source book** — every finding must reference which book and principle it comes from.
- **Don't nitpick design system issues when no system.md exists** — focus on UX heuristics only.
- **Do NOT make changes** — this command only reports findings. The user decides what to fix.
- **Skip shadcn primitives** — files in `components/ui/` are third-party primitives, not review targets.

## Arguments

If a `<path>` argument is provided, review that specific file or all UI files in that directory.

If `--changed` is provided, run `git diff --name-only` and filter for UI files (`.tsx`, `.jsx`, `.vue`, `.blade.php`, `.erb`).

If `--staged` is provided, run `git diff --staged --name-only` with the same filter.

If `--category=<name>` is provided, apply only heuristics from that category:
- `nav` → NAV-01 through NAV-07
- `cognitive` → COG-01 through COG-07
- `visual` → VIS-01 through VIS-06
- `forms` → FRM-01 through FRM-07
- `feedback` → FBK-01 through FBK-05
- `consistency` → CON-01 through CON-05
- `accessibility` → ACC-01 through ACC-05
- `delight` → DLT-01 through DLT-06

If no arguments are provided, glob for common UI paths and review up to 10 files.
