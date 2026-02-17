# Don't Make Me Think — Steve Krug

A deep-dive reference for UX code review through the lens of Steve Krug's foundational usability book. Use this when running `/ux-review:deep krug`.

---

## Core Thesis

**"Don't make me think"** is the first law of usability. Every time a user has to stop and figure something out — even for a fraction of a second — it chips away at their confidence and patience. Self-evident design is the goal. Self-explanatory design is the acceptable fallback. Making users think is the failure mode.

The standard Krug applies: **"It should be self-evident. Obvious. Self-explanatory. The average user should be able to 'get it' without spending any effort thinking about it."**

---

## Key Principles for Code Review

### 1. Self-Evident Design

**The test:** Can a user look at a page and immediately understand what it is, what they can do, and where to click — without having to think about it?

**What to look for in code:**
- **Clickable elements that don't look clickable.** `onClick` handlers on plain `<div>` or `<span>` elements without cursor, hover state, or visual affordance. Links styled as plain text. Buttons that look like labels.
- **Labels that require interpretation.** Nav items, button labels, or headings that use internal jargon, abbreviations without tooltips, or clever wordplay instead of clear descriptions.
- **Ambiguous icons without labels.** Icon-only buttons where the icon meaning isn't universally understood. Krug's rule: if you need a tooltip to explain what it is, either add a text label or change the icon.
- **Forms with no visible labels.** Placeholder-only inputs where the label disappears on focus. The user types two characters and can't remember what field they're filling in.

**Review questions:**
- Would a user who has never seen this app before understand what this page/component does in 5 seconds?
- Are there elements that look interactive but aren't, or look static but are interactive?
- Would you need to explain any part of this UI to a new user?

### 2. How People Actually Use the Web

Krug's three facts:
1. **We don't read pages — we scan them.** Users' eyes dart around looking for keywords, visual cues, and patterns that match their goal.
2. **We don't make optimal choices — we satisfice.** Users click the first reasonable option, not the best one.
3. **We don't figure out how things work — we muddle through.** Users develop mental models that are often wrong but functional.

**What to look for in code:**
- **Scannable content.** Are headings distinct? Are key terms bold or visually emphasized? Or is everything the same weight and size, creating a wall of text?
- **Visual hierarchy.** Is there a clear primary action? Or are multiple actions competing at the same prominence level? Count the buttons in any action bar — if there are more than 3 at the same visual weight, the user has to think about which one to click.
- **Forgiveness for wrong choices.** If a user clicks the wrong thing (and they will), can they easily recover? Back buttons, undo, cancel, breadcrumbs.

### 3. Navigation

Krug frames navigation as the single biggest source of user confusion. If users don't know where they are, where they can go, and how to get back — nothing else matters.

**The navigation must answer:**
- Where am I? (current location highlighted)
- Where can I go? (visible options)
- How did I get here? (breadcrumbs or visual trail)
- How do I get back? (back button, home link)

**What to look for in code:**
- **Active state in navigation.** Is the current page/section visually highlighted in the sidebar or tabs? Look for conditional className logic that applies active styling. If there's none, the user is lost.
- **Breadcrumbs on nested pages.** If the app has more than 2 levels of hierarchy, are there breadcrumbs? Deeply nested pages (e.g., Company → Carrier → Work Order) need wayfinding.
- **Persistent navigation.** Does the sidebar/header disappear on certain pages? Are there routes that hide the main navigation? Users lose context when navigation vanishes.
- **Home/logo link.** Can the user always get back to the starting point? Is the logo/app name a link to home?

### 4. The Trunk Test

Krug's trunk test: imagine you've been blindfolded and driven around, then dropped on a random page of the website. Can you answer:
- What site is this? (identity)
- What page am I on? (page title)
- What are the major sections? (navigation)
- What are my options at this level? (local navigation)
- Where am I in the scheme of things? (breadcrumbs/"you are here")
- How can I search? (search)

**Apply this to every page in the review.** Open the code for any page component and check: does it render all of these orientation elements?

### 5. The Billboard Test

**Text should be scannable at a glance** — like reading a billboard at 60mph.

**What to look for:**
- **Heading clarity.** Do page titles and section headers clearly communicate what this section is about? Or are they vague ("Overview," "Details," "Settings")?
- **Key information prominence.** Is the most important piece of information (the thing the user came for) the most visually prominent element? Or is it buried in a paragraph or a secondary column?
- **Action label clarity.** Do buttons say what they do? "Submit" is generic. "Create Work Order" is clear. "Go" is terrible.

### 6. Usability Testing Mindset

Even when reviewing code (not conducting user tests), think like a usability tester:
- **Watch for the "happy path" bias.** Does the code only handle the ideal flow? What about empty states, error states, partial data, long text that overflows?
- **Look for "we'll figure it out" moments.** Places where the developer assumed users would understand something that actually requires explanation.
- **Find the omissions.** What's NOT in the code that should be? Missing loading states, missing empty states, missing error handling, missing confirmation dialogs.

---

## Krug's Usability Checklist for Code Review

1. Is there a clear visual hierarchy on every page?
2. Are clickable things obviously clickable?
3. Is navigation persistent and clear?
4. Does the user always know where they are?
5. Are form labels visible (not just placeholders)?
6. Are error messages human-readable?
7. Can the user always go back or undo?
8. Is the most important action the most prominent?
9. Are pages scannable (not walls of text)?
10. Would a new user understand this page in 5 seconds?

---

## Common Findings by Component Type

### Pages
- No visual hierarchy (everything same size/weight)
- Navigation doesn't show current location
- Page title is vague or duplicated

### Forms
- Placeholder-only labels
- Submit button says "Submit" instead of what it does
- No inline validation — errors only shown on submit

### Modals
- No close button or escape handler
- Title doesn't explain what this modal is for
- Actions labeled "OK" / "Cancel" instead of specific verbs

### Tables
- Column headers that are abbreviations without explanation
- No empty state when table has no data
- Clickable rows with no visual affordance (no hover state, no cursor)

### Empty States
- Generic "No data" message
- No guidance on what the user should do next
- No visual element (icon, illustration) to soften the blank page
