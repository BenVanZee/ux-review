---
name: ux-review:deep
description: Deep-dive UX review using a specific book's full reference or a cross-book topic lens. Loads the complete reference file for thorough analysis.
---

# UX Deep Dive

Review UI code through the lens of a specific book or topic.

## Usage

### By book:
```
/ux-review:deep krug <path>           # Don't Make Me Think
/ux-review:deep rocket <path>         # Rocket Surgery Made Easy
/ux-review:deep laws <path>           # Laws of UX
/ux-review:deep cognitive <path>      # Design for How People Think
/ux-review:deep patterns <path>       # Designing Interfaces
/ux-review:deep emotion <path>        # Designing for Emotion
```

### By topic (cross-book):
```
/ux-review:deep forms <path>          # Form UX across all books
/ux-review:deep navigation <path>     # Navigation across all books
/ux-review:deep tables <path>         # Table & data collection patterns
```

If `<path>` is omitted, prompt the user for a target file or directory.

## Process

### Book-Specific Deep Dive

1. **Read the ux-review SKILL.md** — load heuristics and output format
2. **Read the corresponding reference file** from `references/`:
   - `krug` → `references/dont-make-me-think.md`
   - `rocket` → `references/rocket-surgery.md`
   - `laws` → `references/laws-of-ux.md`
   - `cognitive` → `references/design-for-how-people-think.md`
   - `patterns` → `references/designing-interfaces.md`
   - `emotion` → `references/designing-for-emotion.md`
3. **Read `.interface-design/system.md`** if present
4. **Read the target file(s)**
5. **Apply that book's full lens** — use every principle, checklist, and review question from the reference file. This is deeper than a standard review.
6. **Output findings** using the standard format from SKILL.md, but cite the specific book chapters/sections extensively

### Topic-Specific Deep Dive

For cross-book topics, load multiple reference files and apply the relevant sections from each:

#### `forms`
Load all 6 references. Apply:
- Tidwell: Form patterns (forgiving format, structured format, good defaults, input hints)
- Krug: Scannable forms, self-evident fields, placeholder-only labels
- Laws: Hick's Law (too many fields), Fitts's Law (submit button placement), Postel's Law (forgiving input)
- Whalen: Memory & knowledge (recognition vs recall), decision making (smart defaults)
- Walter: Error recovery empathy, anxiety reduction on submission
- Rocket: Common form problems and quick fixes

Focus on FRM-01 through FRM-07, plus relevant heuristics from FBK, COG, ACC, and DLT.

#### `navigation`
Load all 6 references. Apply:
- Krug: Trunk test, persistent navigation, "you are here"
- Tidwell: Hub-and-spoke, pyramid, tabs, modal panel patterns
- Laws: Hick's Law (too many nav items), Miller's Law (grouping)
- Whalen: Wayfinding, spatial mental maps
- Rocket: "Users get lost" — common navigation problems

Focus on NAV-01 through NAV-07, plus COG-01, COG-02, CON-05.

#### `tables`
Load the `designing-interfaces.md` reference (which has the full table deep-dive) plus relevant sections from others. Apply:
- Tidwell: Table patterns, column design, sorting, filtering, pagination, row interaction, action columns, empty states, mobile behavior, cross-table consistency
- Krug: Scannable tables, self-evident column headers, search findability
- Laws: Miller's Law (column count), Hick's Law (too many actions per row), Fitts's Law (click targets in cells)
- Whalen: Vision & attention (column hierarchy), memory (recognizable patterns across tables)
- Rocket: Missing features that cause user confusion
- Walter: Empty state personality, table loading states

Focus on VIS, COG, CON, NAV-07 (search), and ACC heuristics as they apply to tables.

## Output

Use the same format as `/ux-review:review` but with:
- **Deeper analysis** — more context per finding, referencing specific book chapters
- **Book-specific insights** — observations that only emerge from that book's particular lens
- **Header indicates the lens:** `## UX Deep Dive: [book/topic] lens on [scope]`

## Important

- **This is a thorough review**, not a quick scan. Read every line of the target files.
- **Cite extensively.** Reference specific chapters, sections, and principles from the book.
- **Include the full review checklist** from the reference file in your analysis.
- **Do NOT make changes** — report only.
