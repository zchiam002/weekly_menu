---
name: weekly-menu
description: Plan a weekly dinner menu (Mon–Fri) and propagate it through the kitchen workflow — week file, recipes, market list, Jayzel's pantry questions, market list confirmation, step-by-step cooking instructions per dish, dish rotation, inventory. Use when the user mentions "this week's menu", "next week's menu", "plan menu", "weekly menu", or "menu for the week".
---

# Weekly menu planning

User plans Mon–Fri dinners for ~5.5 adults. Jayzel is the household helper (since 3 Oct 2026; Mia before). All artifacts live in `D:\ZLStuff\development\weekly_menus\`.

Follow these 12 steps in order. After any step that asks for user input, **wait for the user's confirmation before moving on** — do not skip ahead.

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

Default scale: **5.5 adults** (Mon–Fri). **Sunday dinners are always 3 pax** (standing rule from the user,
17 Oct 2026). Confirm if the user implies a different number.

**Household portions** (standing rules from the user, 18 Oct 2026):
- Chicken mid wings: **2 per person**.
- Fishballs: **3 per person, rounded to the nearest even number**. The helper buys them fresh early on
  the morning they are cooked.

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

## Step 6 — Questions to Jayzel

Generate a numbered pantry-check list to send to Jayzel via WhatsApp. Cover:
- Specialty items (curry pastes, wasabi, dashi, broad bean paste, etc.)
- Quantity-sensitive staples (oils, sauces, butter, baking soda)
- Items used heavily this week (minced pork, eggs, ginger, garlic)
- Frozen proteins likely already in stock

Cross-reference `kitchen-inventory.xlsx` Inventory tab — anything at Status `Low` or `Out` should be on this list.

Format:

```
Questions to Jayzel:
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
- 1 loaf of bread (helper's breakfast for the week)

Breakfast
- wife's breakfast
- kid's breakfast
- tomorrow's breakfast (the morning after the shop)

Cold Storage  (only if premium items needed)
- ...
```

**Always add these to every market list** (standing rule from the user, 13 Oct 2026):

1. **Breakfast for the wife and the kid, plus the next day's breakfast.** Give the list a `Breakfast`
   section with three parts: the wife's, the kid's, and the morning after the shop. The items
   change from week to week, so **ask the user what they want** — do not invent them. Past lists
   have carried bananas, lo mai gai, kaya toast, pau, blueberries, Pediasure and Milo; offer those
   as suggestions only.
2. **1 loaf of bread for the helper's weekday breakfast.** Put it under Supermarket on every list.

Also, **list chicken thighs by number** ("3 chicken thighs"), never by weight — the user dislikes
weights for thighs (standing rule, 17 Oct 2026). And **coconut milk is always Kara, in 65ml or 200ml
packs** — never cans: round a recipe's amount to whole packs (400ml = 2 × 200ml; 150ml = 1 × 200ml;
50ml = 1 × 65ml) (standing rule, 17 Oct 2026).

## Step 8 — Confirm the market ingredients

**Stop here and wait.** Present the Step 7 list as a WhatsApp message (plain text, `*bold*` section
labels, one `- item — for <dish>` line each). The user usually replies with their **own final list**.

- Their list is the source of truth. Keep their quantities and wording exactly; never silently
  change them.
- Reconcile it against the Step 3 per-dish quantities and **flag, don't fix**: items that are short
  of what a recipe needs, ingredients a dish needs that are neither on the list nor assumed in the
  pantry, items no dish uses, and anything that depends on a choice the user has not made yet.
- Save the confirmed list at the top of `YYYY-MM-DD-market.md` as FINAL (dated, "user's final
  list"), keep the flagged gaps below it, and keep the earlier draft underneath for the per-dish
  quantities.
- Ask whether the list is confirmed. **Do not start Step 9 until the user says it is.** If they
  change the list again, update the file and re-flag.

Everything after this point (Steps 9–12) works from the confirmed list, not the draft.

## Step 9 — Step-by-step instructions for every dish

Runs once the market list is confirmed (Step 8). Write a numbered, step-by-step cooking list for
**each and every dish in the menu** — mains, soups, sides and plain staples (steamed rice, stir fried
vegetable, prata) alike — with the video next to it, so the helper can cook from the message alone.

Rules:
- **Source:** `recipes/<dish>.md`. Use the file's own method with this week's **scaled amounts written
  into the steps** (e.g. "Add 3 tbsp light soy sauce"), not the recipe's original amounts. Metric
  only — g, kg, ml, °C, cm (tbsp and tsp are fine); convert cups, inches and °F. Pre-scaled files
  are used with the amounts as they stand.
- **Video:** where our amounts differ from the video's, add a one-line note after the dish name
  (e.g. "video serves 3–4, we make 1.6×"). **Finish each day's message with a `*Videos*` section**
  (user, 18 Oct 2026): the dish name on one line, its source URL on the next line so WhatsApp
  auto-links it, then a `No video:` line naming the dishes that have none.
- **No recipe file, or a file with ingredients only and no method:** do not invent steps. Say so
  ("No written method — follow the video") and give the link. A household dish with no video is
  "Household recipe — <one line>". Give estimated steps only when the user has allowed estimates,
  and mark them "(estimate)".
- **Chillies, every dish (standing rule, 18 Oct 2026):** remove all the chilli seeds, and tell the helper
  to wear gloves when handling chillies. Put both in the steps and list "Gloves" under `You need:`.
- **Beans, every dish (standing rule, 18 Oct 2026):** long beans and French beans — cut the ends off
  and throw them away, blanch the beans whole in hot water, drain, and only then cut them up.
- **Ingredients in point form:** under each dish write `You need:` and then one ingredient per line,
  each starting with `- `. Never a single run-on line (user, 18 Oct 2026).
- **Adjustments** agreed in the week file and the vet (chilli reduced, wine added early, raw garnish
  left off, cream left out) are written into the steps, not left as side notes.
- **Doneness and safety checks** go in as plain cooking steps ("cut the thickest piece open: no
  pink, juices clear"). **Never mention pregnancy** — this goes to the helper.
- **Do-ahead items** (thaw, soak, cook rice the night before, marinate) open each day's message
  as a one-line "Do ahead".
- **Keep Chinese dish names.**
- **Save them:** write the final day messages to `YYYY-MM-DD-instructions.md` at the project root, one
  section per day with the message in a code block, and update the file whenever the user changes a
  day (user, 18 Oct 2026).
- **Format:** plain text, no markdown headers; `*bold*` for the day and dish names; `1. 2. 3.` for
  the steps. **One message per day** (Sunday extra first). Wrap each message in a code block when
  presenting it.

Format per day:

```
*<Day DD Mmm> — step by step*
Do ahead: <only if something needs a lead time>

*1. <Dish>* (<short note, e.g. "makes 1.6x the video">)
You need:
- <ingredient>
- <ingredient>
1. <step>
2. <step>

*2. <Dish>*
You need:
- <ingredient>
1. <step>
2. <step>

*Videos*
<Dish 1>
<video url>
<Dish 2>
<video url>
No video: <dishes with none>
```

## Step 10 — Update rotation tracker

Append rows to `dish-rotation.xlsx` → "Cook log" tab. One row per dish per day:
- Date (Monday = week's start; Tuesday = Monday + 1; etc.)
- Day (formula auto-fills)
- Dish (must match exactly to a row in "Dish rotation" master)
- Notes (e.g., "planned (week of DD Mmm YYYY)")

If a dish in the menu is not in the "Dish rotation" master, add it there first with a sensible Category and Cool-down (default 21 days; specials/elaborate dishes 28–35; weekday anchors 14).

Use Python + openpyxl (the workbook has formulas — use `load_workbook` to preserve them, append rows, `save`). **Do not ask the user to do it manually.**

## Step 11 — Update inventory

For any **new** ingredient introduced this week (a specialty item not yet in the Inventory tab, including anything the user added to the confirmed list in Step 8), add a row to `kitchen-inventory.xlsx` → "Inventory" tab with Status = `Out` (it'll be on the shopping list).

**Do not change existing Status values** — those reflect Jayzel's reality. Only the user/Jayzel changes status of existing items.

## Step 12 — WhatsApp messages

After Steps 1–11 are complete, generate four ready-to-paste WhatsApp messages. All are **plain text, no markdown headers**. Use WhatsApp's inline formatting: `*bold*`, `_italic_`. URLs on their own line so they auto-link.

Wrap each message in a code block (triple backticks) when presenting to the user — that way they can copy the contents without the code-block markers being applied.

### 12a — Menu (for general sharing or pinning)

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

### 12b — Cooking notes to Jayzel

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

### 12c — Recipe links to Jayzel

One section per dish, day-labelled. Source URL on its own line (so WhatsApp auto-links). For dishes with no URL (household / family recipes), state that explicitly so Jayzel knows to ask for the method. Step 9's instructions already carry each video next to its steps, so keep this as the short day-by-day links list unless the user says to drop it.

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

### 12d — Reassuring message to spouse (only if pregnancy-vet was run)

A personal message from the user to their partner summarising the menu and what's been adjusted for pregnancy safety. Tone: warm, brief, specific, not clinical. Empower the spouse to redirect ("anything you want swapped, just tell me").

This message **can** name the pregnancy considerations directly (Western / TCM cautions, blood-tonifying ingredients, etc.) — it's between the couple, not for Jayzel.

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

<one-line warm close noting Jayzel has the cooking notes and offering to swap anything>
```

Skip 12d entirely if the pregnancy-vet skill was not invoked for this week.

---

## Files reference

- Week file: `YYYY-MM-DD.md` at project root (Monday's date)
- Recipes: `recipes/<kebab-case>.md`
- Rotation tracker: `dish-rotation.xlsx`
- Inventory: `kitchen-inventory.xlsx`
- Step-by-step instructions: `YYYY-MM-DD-instructions.md` — one WhatsApp message per day (Step 9)
- Market list: `YYYY-MM-DD-market.md` — the user's confirmed (FINAL) list at the top, the flagged gaps
  below it, then the draft and the per-dish quantities
- History archive: `history.md`

## Conventions

- Household: ~5.5 adults (default scale)
- Jayzel = helper; pantry questions go to WhatsApp
- WhatsApp-paste output: plain text only, no markdown
- Preserve Chinese names where present (e.g., `鱼香茄子`)
- Use ISO dates for week files (`YYYY-MM-DD`)
- Use kebab-case for recipe filenames
- Market list: the user's own list is final; flag gaps, never overwrite their quantities
- Cooking instructions (Step 9): one WhatsApp message per day, video link under each dish, scaled
  metric amounts, no invented steps
