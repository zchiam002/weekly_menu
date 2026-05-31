---
name: weekly-menu
description: Plan a weekly dinner menu (Mon–Fri) and propagate it through the kitchen workflow — week file, recipes, market list, Mia's pantry questions, dish rotation, inventory. Use when the user mentions "this week's menu", "next week's menu", "plan menu", "weekly menu", or "menu for the week".
---

# Weekly menu planning

User plans Mon–Fri dinners for ~5.5 adults. Mia is the household helper. All artifacts live in `D:\ZLStuff\development\weekly_menus\`.

Follow these 9 steps in order. After any step that asks for user input, **wait for the user's confirmation before moving on** — do not skip ahead.

## Step 1 — Initial plan

Propose a Mon–Fri menu (5 days, 2–4 dishes per day).

Selection rules:
- Read `dish-rotation.xlsx` → "Dish rotation" tab.
- Prefer dishes with Status = `Due` or `Never cooked`. Avoid `Cooldown` unless the user asks.
- Balance proteins across the week — don't pile up pork/chicken/fish/prawn.
- Mix cooking methods: steamed / stir-fry / braised / soup / noodle / rice / oven.
- Respect anchors the user has mentioned in past weeks (e.g., fried rice often Mon, ee-fu mee often Wed).

Output format:

```
## Monday (DD Mmm)
1. Dish
2. Dish

## Tuesday (DD Mmm)
...
```

## Step 2 — Refine via chat

The user will swap, add, or remove dishes. Iterate freely. **Do not move to step 3 until the user explicitly confirms the menu is final.**

Once final, save the menu to `YYYY-MM-DD.md` (Monday's date) at the project root. Format:

```
# Weekly Menu — Week of DD Mmm YYYY

## Monday (DD Mmm)
1. Dish
2. Dish
...
```

## Step 3 — Ingredient list

Default scale: **5.5 adults**. Confirm if the user implies a different number.

For each dish:
- Read `recipes/<dish>.md` (kebab-case filename).
- Scale factor = 5.5 / recipe's stated servings.
- Apply factor and round sensibly: "1.83 × 3 cloves" → "5–6 cloves", not "5.5 cloves".

Output organised by day, dish-by-dish.

## Step 4 — Recipe links

For each dish in the menu, check `recipes/`:
- **File exists** — use it.
- **File missing** — ask the user for the YouTube link or other recipe source. **Do not fabricate a recipe from memory.**

## Step 5 — Update or create recipe files

- **Update** (user changed an existing recipe): edit `recipes/<dish>.md`.
- **Create** (new dish): write `recipes/<kebab-case-name>.md`. Match the existing structure — H1 title, `**Servings:**` line, ingredient sections (use `###` subsections like Marinade / Gravy / Sauce when natural), source link at bottom. Preserve Chinese names where present (e.g., `鱼香茄子`).

## Step 6 — Questions to Mia

Generate a numbered pantry-check list to send to Mia via WhatsApp. Cover:
- Specialty items (curry pastes, wasabi, dashi, broad bean paste, etc.)
- Quantity-sensitive staples (oils, sauces, butter, baking soda)
- Items used heavily this week (minced pork, eggs, ginger, garlic)
- Frozen proteins likely already in stock

Cross-reference `kitchen-inventory.xlsx` Inventory tab — anything at Status `Low` or `Out` should be on this list.

Format:

```
Questions to Mia:
1. How much X do we have left?
2. Do we have Y?
...
```

## Step 7 — Consolidate market list

Format matching the user's history:

```
Market
- proteins (with weights)
- fish
- fresh produce

Supermarket
- packaged / shelf-stable
- specialty Asian items

Cold Storage  (only if premium items needed)
- ...
```

## Step 8 — Update rotation tracker

Append rows to `dish-rotation.xlsx` → "Cook log" tab. One row per dish per day:
- Date (Monday = week's start; Tuesday = Monday + 1; etc.)
- Day (formula auto-fills)
- Dish (must match exactly to a row in "Dish rotation" master)
- Notes (e.g., "planned (week of DD Mmm YYYY)")

If a dish in the menu is not in the "Dish rotation" master, add it there first with a sensible Category and Cool-down (default 21 days; specials/elaborate dishes 28–35; weekday anchors 14).

Use Python + openpyxl (the workbook has formulas — use `load_workbook` to preserve them, append rows, `save`). **Do not ask the user to do it manually.**

## Step 9 — Update inventory

For any **new** ingredient introduced this week (a specialty item not yet in the Inventory tab), add a row to `kitchen-inventory.xlsx` → "Inventory" tab with Status = `Out` (it'll be on the shopping list).

**Do not change existing Status values** — those reflect Mia's reality. Only the user/Mia changes status of existing items.

## Step 10 — WhatsApp messages

After Steps 1–9 are complete, generate four ready-to-paste WhatsApp messages. All are **plain text, no markdown headers**. Use WhatsApp's inline formatting: `*bold*`, `_italic_`. URLs on their own line so they auto-link.

Wrap each message in a code block (triple backticks) when presenting to the user — that way they can copy the contents without the code-block markers being applied.

### 10a — Menu (for general sharing or pinning)

Format:

```
*Week of DD Mmm YYYY dinner menu* 🍽️

Note: <any heads-up like "Mon is a public holiday — eating out, no cook">

*Sun DD Mmm*
- Dish
- Dish

*Mon DD Mmm*
- ...
```

🍽️ emoji is conventional for this user — keep it.

### 10b — Cooking notes to Mia

A condensed version of the prep tips that came out of the pregnancy vet (if run) plus any other dish-specific notes (substitutions, prep order, doneness checks).

**Do not mention pregnancy directly** — translate each pregnancy-related caution into a generic cooking instruction. Reason: per memory, the helper should receive instructions as standing preferences, not as pregnancy-specific. Examples:
- "Substitute sake with chicken broth" (don't say "to avoid alcohol in pregnancy")
- "Char siew must be reheated thoroughly" (don't say "listeria risk")
- "Reduce bird's eye chillies" (don't say "TCM heaty for T1")

Format:

```
*Cooking notes — Week of DD Mmm*

*<day> <dish>*
- <instruction>
- <instruction>

*<day>*
- <instruction>
```

### 10c — Recipe links to Mia

One section per dish, day-labelled. Source URL on its own line (so WhatsApp auto-links). For dishes with no URL (household / family recipes), state that explicitly so Mia knows to ask for the method.

Format:

```
*Recipe links — Week of DD Mmm*

*<day> — <dish>* (any short note like "skip wine" or "with X")
<url>

*<day> — <dish>*
Household recipe — <one-line description>

*<day> — <dish>*
<url>
```

### 10d — Reassuring message to spouse (only if pregnancy-vet was run)

A personal message from the user to their partner summarising the menu and what's been adjusted for pregnancy safety. Tone: warm, brief, specific, not clinical. Empower the spouse to redirect ("anything you want swapped, just tell me").

This message **can** name the pregnancy considerations directly (Western / TCM cautions, blood-tonifying ingredients, etc.) — it's between the couple, not for Mia.

Format:

```
Hey love,

<one-line summary of what's been planned and that it's been vetted>

*Menu*
- Sun: ...
- Mon: ...
...

*What I've adjusted*
- <substitution / preparation note>
- <substitution / preparation note>

<one-line warm close noting Mia has the cooking notes and offering to swap anything>
```

Skip 10d entirely if the pregnancy-vet skill was not invoked for this week.

---

## Files reference

- Week file: `YYYY-MM-DD.md` at project root (Monday's date)
- Recipes: `recipes/<kebab-case>.md`
- Rotation tracker: `dish-rotation.xlsx`
- Inventory: `kitchen-inventory.xlsx`
- History archive: `history.md`

## Conventions

- Household: ~5.5 adults (default scale)
- Mia = helper; pantry questions go to WhatsApp
- WhatsApp-paste output: plain text only, no markdown
- Preserve Chinese names where present (e.g., `鱼香茄子`)
- Use ISO dates for week files (`YYYY-MM-DD`)
- Use kebab-case for recipe filenames
