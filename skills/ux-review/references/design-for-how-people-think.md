# Design for How People Think — John Whalen

A deep-dive reference for UX code review through the lens of John Whalen's cognitive psychology approach to UX design. Use this when running `/ux-review:deep cognitive`.

---

## Core Thesis

Users don't experience your interface logically — they experience it **cognitively**. Every interaction is filtered through perception, attention, memory, mental models, and emotion. Good UX doesn't fight these cognitive realities — it works with them.

Whalen identifies **6 cognitive dimensions** (the "6 Minds") that shape every user experience. Code review through this lens looks for places where the UI conflicts with how the brain actually processes information.

---

## The 6 Minds — Applied to Code Review

### 1. Vision & Attention

**How it works:** The visual system processes information in layers — first broad patterns (layout, color, size), then focal details (text, icons, specific elements). Attention is scarce and involuntary — it's drawn to contrast, movement, and novelty.

**What to look for in code:**

- **No visual hierarchy.** Pages where all elements have similar size, weight, and color. The eye doesn't know where to start. Look for pages with no `text-xl` or larger headings, no bold/semibold emphasis, no color differentiation.
- **Competing attention grabbers.** Multiple elements with bright colors, large sizes, or animations. When everything screams for attention, nothing gets it. Count the number of "accent" or "primary" colored elements on a single view — more than 2-3 is a red flag.
- **Important information in low-contrast areas.** Key data rendered in `text-muted-foreground` or `text-gray-400`. Status indicators using only subtle color differences. Critical alerts with no visual prominence.
- **Motion misuse.** Animations that distract from the primary content. Loading spinners that draw more attention than the content they're loading. Animated elements in the periphery while the user is trying to focus.

**Review questions:**
- If I squint at this page, what stands out? Is that the right thing?
- What's the first thing a user would look at? Is that the most important thing?
- Are there visual competitors fighting for the same attention?

### 2. Wayfinding

**How it works:** Humans constantly build and maintain spatial mental maps. In a physical space, landmarks, signs, and paths help us navigate. In a digital space, navigation, breadcrumbs, and consistent layout serve the same function.

**What to look for in code:**

- **No landmarks.** Pages that all look the same — same layout, same structure, no visual distinction between sections. Users can't build a mental map if everything looks identical.
- **Inconsistent page structure.** Content area layout changes between pages — sometimes left-aligned, sometimes centered, sometimes cards, sometimes raw content. Users expect positional consistency.
- **Missing "you are here" indicators.** Navigation without active states. Pages without titles. Modals without context about what record they're editing.
- **Disorienting transitions.** Navigation that changes the entire page layout. Modals that open modals. Multi-step flows that change the surrounding UI.

### 3. Memory & Knowledge

**How it works:** Working memory holds 4-7 items for about 20 seconds. Long-term memory is associative, not indexical — users remember things by connection, not by position. Recognition is always easier than recall.

**What to look for in code:**

- **Recall over recognition.** Text inputs where dropdowns/selects would work (user must remember exact values). Instructions that reference UI elements not visible on screen ("go to the Settings page and click..."). Form fields that require information the user must look up elsewhere.
- **Working memory overload.** Multi-step forms where information from step 1 isn't visible on step 3. Comparison features that require switching between pages. Reference data (codes, IDs) that must be mentally held while performing another action.
- **No learning support.** First-time experiences identical to repeat experiences — no tooltips, no onboarding hints, no empty-state guidance.

**Code-level checks:**
- Are there `<input type="text">` elements that could be `<Select>` components (fixed set of values)?
- Do multi-step forms show a summary of previous steps?
- Do empty states provide guidance, or just say "No items"?

### 4. Language & Semantics

**How it works:** Words are not just labels — they carry mental models, emotional weight, and cognitive associations. Users read labels and headings to build expectations. When the content doesn't match the label, confusion results.

**What to look for in code:**

- **Jargon without context.** Internal terminology, acronyms, or industry-specific terms used in UI labels without explanation. "FMCSA" or "DOT" might be obvious to domain users but "MC Authority" vs. "Operating Authority" might not be.
- **Inconsistent terminology.** Same concept called different things on different pages (see CON-05). The user builds a mental model around one term — seeing another term for the same thing forces re-interpretation.
- **Labels that mislead.** Button that says "Save" but actually creates a new record. "Delete" button that archives instead of deleting. "Close" that discards unsaved changes without warning.
- **Passive or vague microcopy.** "An error occurred" (what error?). "Are you sure?" (sure about what?). "Processing..." (processing what?).

### 5. Decision Making

**How it works:** Users don't make optimal decisions — they use heuristics (mental shortcuts). They satisfice (pick the first "good enough" option), are influenced by defaults, and are strongly affected by how choices are framed.

**What to look for in code:**

- **No default selections.** Form fields with no default value when a sensible default exists. Date pickers defaulting to an unhelpful date. Required selections with "Choose one..." as the only placeholder.
- **Poor choice framing.** Destructive confirmations that frame both options neutrally ("Cancel" / "OK") instead of clearly ("Keep Editing" / "Delete Work Order").
- **Decision overload.** Too many options at once (see Hick's Law). Settings pages showing everything. Filters with many dimensions and no suggested presets.
- **No recommendation or guidance.** When multiple options exist and one is clearly better for most users, the UI should nudge toward it. Plans without a "recommended" tag. Options without descriptions of trade-offs.

### 6. Emotion

**How it works:** Emotion isn't separate from cognition — it's integral. Frustration makes tasks harder. Delight makes friction tolerable. Anxiety makes users abandon. Confidence makes users explore.

**What to look for in code:**

- **Anxiety-inducing moments without reassurance.** Destructive actions with no "what happens if I do this?" context. Long forms with no save indicator. Payment forms with no security indicators. Submit buttons that disable with no loading feedback.
- **Frustration amplifiers.** Lost data after navigation. Long forms that reset on error. Validation that only runs on submit (user fills 20 fields, clicks submit, finds 3 errors, has to scroll up to find them).
- **No acknowledgment of effort.** User completes a complex multi-step flow and gets a one-line toast. Long data entry with no progress indication. Bulk operations with no completion summary.
- **No personality.** Every message is clinical and robotic. No warmth in empty states. No humor or charm anywhere appropriate. (See DLT heuristics for specifics.)

---

## Cognitive Load Analysis Framework

When reviewing a component, assess cognitive load across three types:

### Intrinsic Load (the task itself)
- Is this task inherently complex? (Filing IFTA returns vs. clicking a button)
- Has the UI reduced unnecessary complexity while preserving necessary complexity?
- Are complex tasks broken into manageable steps?

### Extraneous Load (the UI getting in the way)
- Is the user spending effort on the interface rather than the task?
- Are there unnecessary steps, clicks, or page loads?
- Is information the user needs readily visible, or hidden behind interactions?
- Does the layout require visual searching rather than guiding the eye?

### Germane Load (productive learning)
- Does the UI help the user build a useful mental model?
- Are patterns consistent enough that the user learns them quickly?
- Do labels and categories match the user's domain knowledge?
- Are there affordances that teach without requiring explicit instruction?

**The goal:** Minimize extraneous load (UI friction), keep intrinsic load appropriate (don't oversimplify domain complexity), and support germane load (help users learn the system).

---

## Review Checklist by Cognitive Dimension

| Dimension | Question | Finding if No |
|-----------|----------|---------------|
| Vision | Does the visual hierarchy match the content hierarchy? | VIS-01, VIS-04 |
| Vision | Is there one clear focal point per view? | COG-01, VIS-01 |
| Wayfinding | Can the user always identify where they are? | NAV-01 |
| Wayfinding | Is page structure consistent across similar pages? | CON-03 |
| Memory | Are recognition-based inputs used instead of recall? | COG-04 |
| Memory | Is working memory load kept under 7 items? | COG-02 |
| Language | Is terminology consistent throughout? | CON-05 |
| Language | Are labels specific and accurate? | NAV-02 |
| Decision | Are sensible defaults provided? | COG-07 |
| Decision | Are choices framed to guide good decisions? | COG-01 |
| Emotion | Is anxiety reduced at high-stakes moments? | DLT-04 |
| Emotion | Does effort receive proportional acknowledgment? | FBK-05, DLT-01 |

---

## Common Cognitive UX Issues by Component Type

### Forms
- Too many fields visible at once (working memory overload)
- Text inputs where selects would work (recall vs. recognition)
- No smart defaults (user has to think about every field)
- Validation only on submit (frustration amplifier)

### Tables/Lists
- Too many columns (visual processing overload)
- No sorting/filtering (user must visually scan entire list)
- Inconsistent row structure (can't build pattern expectations)

### Modals
- No context about the parent record (wayfinding failure)
- Too much content for a modal (wrong container for the complexity)
- Actions labeled generically (decision framing failure)

### Navigation
- Too many top-level items (working memory overload)
- No active state (wayfinding failure)
- Labels that don't match page content (language/semantics failure)

### Dashboard
- All metrics at equal visual weight (attention competition)
- No hierarchy in information architecture (wayfinding failure)
- Numbers without context (what does "47" mean? good? bad? out of how many?)
