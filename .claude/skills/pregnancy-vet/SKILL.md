---
name: pregnancy-vet
description: Vet a weekly menu or market list for pregnancy safety, applying both Western OB-GYN food-safety guidelines and Traditional Chinese Medicine (TCM) considerations. Use when the user mentions "pregnancy vet", "pregnancy safe", "vet for pregnancy", "check menu for pregnancy", "TCM check", or asks to verify a menu/ingredient list against pregnancy guidelines.
---

# Pregnancy-safe menu vet (Western + TCM)

A final-pass check on a planned menu or consolidated market list, run before the list is handed off to Mia or used for shopping. Output is a discussion list — **not medical advice**. Always flag that the user should cross-check with their actual GP and TCM physician.

Follow these 5 steps in order. After any step that asks for user input, **wait for the user's confirmation before moving on**.

## Step 1 — Clarify scope

Before reading any list, confirm with the user:

1. **Which artifact to vet** — the week file (`YYYY-MM-DD.md`), the consolidated market list from the weekly-menu workflow Step 7, or a list the user pastes in. If unclear, ask.
2. **Pregnancy stage** — first / second / third trimester, or "trying to conceive" / "preconception". Cautions shift across stages (e.g., cooling foods are more strictly avoided in T1; blood-tonifying foods become more relevant in T2–T3). If the user doesn't specify, default to **first trimester** (most cautious) and flag the assumption.
3. **Any pre-existing flags from the GP or TCM physician** — e.g., "GP said avoid all raw fish", "TCM said cut back on heaty foods", "gestational diabetes — watch sugar". Apply those on top of the generic guidance.

Do not proceed to Step 2 until these three are answered (or explicitly defaulted).

## Step 2 — Build the dish-level ingredient view

For each dish in the menu:
- Read `recipes/<dish>.md` for the full ingredient list. If the file is missing, ask the user to provide the recipe or skip with a note.
- Note the preparation method (raw, lightly cooked, fully cooked, deep-fried, simmered, etc.) — method matters for food-safety calls (e.g., sashimi vs grilled salmon).

Aggregate to an ingredient set per day so the next step can scan cleanly.

## Step 3 — Apply the Western OB-GYN food-safety lens

Scan each ingredient and preparation against the standard pregnancy avoid-list:

**High-risk avoid (any trimester)**
- Raw / undercooked fish or shellfish (sashimi, ceviche, raw oysters, lightly-cured salmon)
- Raw / undercooked meat or poultry (rare beef, runny minced pork, pink chicken)
- Raw or runny eggs (mayonnaise made with raw egg, hollandaise, soft-boiled, half-cooked tamago)
- Unpasteurised dairy and soft cheeses (Brie, Camembert, blue cheese, feta if unpasteurised)
- Deli meats and pâté (listeria risk unless reheated to steaming)
- High-mercury fish (swordfish, king mackerel, tilefish, shark, large tuna — limit canned light tuna)
- Raw sprouts (alfalfa, mung bean sprouts eaten raw — cooked is fine)
- Unwashed raw produce
- Excess caffeine (coffee, strong tea — usually capped at ~200mg/day)
- Alcohol — including alcohol used in cooking if not fully simmered off (sake, mirin, Shaoxing wine: typically fine when simmered long enough, but flag when added at the end or in glazes that don't reduce)

**Moderate / case-by-case**
- Liver and organ meats (very high vitamin A — typically limit)
- Smoked fish and cold-smoked meats (listeria — fine if heated through in a hot dish)
- Honey (fine for the mother; the avoidance is for infants <12mo, not pregnancy — flag the common misconception only if user asks)
- Cured meats in cooked dishes (acceptable when cooked thoroughly)

Mark each flagged ingredient with its source dish, the issue, and the preparation context (e.g., "claypot tofu — uses Shaoxing wine, added before long simmer → likely fine; confirm").

## Step 4 — Apply the TCM lens

Group ingredients by TCM property and flag those known to be cautioned during pregnancy. Stage matters — note it in each flag.

**Cooling / 寒凉 — typically eased back in T1**
- Crab, raw seafood, raw fish
- Watermelon, pear, persimmon (in excess)
- Mung bean, barley
- Bitter gourd
- Ice / cold drinks
- Excess raw salad

**Heaty / 上火 — moderated throughout, especially with existing 热气 symptoms**
- Deep-fried foods in volume
- Lamb, durian, longan in excess
- Chilli, Sichuan pepper in heavy quantities
- Lychee in excess

**TCM herbs / functional ingredients flagged in pregnancy**
- 山楂 hawthorn — flagged for possible uterine-stimulating effect, often avoided
- 薏米 Job's tears (yi mi / barley) — traditionally avoided
- 红花 safflower, 桃仁 peach kernel, 当归 dang gui (in herbal-medicine dosages) — TCM caution
- 芦荟 aloe vera (oral), 番泻叶 senna — strong purgatives, avoid
- Raw papaya (especially green) — folk caution; ripe papaya in moderation is generally accepted
- Pineapple (very high doses) — folk caution
- Coix seed (yi yi ren / 薏苡仁) — same family as Job's tears, traditionally avoided

**Blood-building / 补血 — typically encouraged T2–T3**
- Red dates (jujube / 红枣)
- Goji berries (枸杞)
- Black sesame
- Liver (but watch the vitamin A ceiling — Western lens overrides here)
- Pork rib soups with longan / red dates

Note any helpful ingredients in the menu — not just the cautions. Tonics in the right stage are a positive signal.

## Step 5 — Output the vet report

Structure the report by dish, then summarise. Each dish gets one of three verdicts:

- **AVOID** — at least one ingredient or preparation falls in the high-risk avoid list. Suggest a substitution if obvious.
- **CAUTION** — borderline ingredient, preparation matters, or TCM-stage concern. Note the specific concern.
- **OK** — no flags raised.

Format:

```
# Pregnancy vet — Week of DD Mmm YYYY
Stage assumed: <trimester>
Pre-existing flags: <list, or "none">

## Sunday — Dish name
Verdict: OK / CAUTION / AVOID
Notes:
- <ingredient or preparation> → <issue> (<Western / TCM>)
- ...
Suggested swap: <only if AVOID>

## Monday — ...
...

## Summary
- Dishes to swap: <list>
- Dishes to adjust preparation: <list>
- Ingredients Mia should flag at the market or in the kitchen: <list>
- Net assessment: <one paragraph>

## Disclaimer
This is a discussion list based on general Western OB-GYN guidance and TCM tradition.
Cross-check anything flagged with the GP and TCM physician before acting on it.
This is not medical advice.
```

Save the report as `pregnancy-vet/YYYY-MM-DD.md` (Monday's date), at the project root, so it can be referenced alongside the week file.

---

## Conventions

- Default stage = **first trimester** if unstated (most cautious).
- Always include the disclaimer.
- Use plain text labels (AVOID / CAUTION / OK) — no emoji.
- Preserve Chinese names of herbs and TCM-flagged ingredients where present.
- When a recipe is missing and the dish can't be vetted, mark it `UNKNOWN — recipe not on file` and ask the user for the source.
- TCM guidance is folk-tradition leaning; when Western and TCM conflict (e.g., honey for cough vs Western "fine for mother"), state both views and let the user decide.

## When to use this skill

- After Step 7 of the weekly-menu workflow (consolidated market list), before passing it to Mia.
- Ad-hoc when the user pastes a menu, a market list, or a single dish and asks for a pregnancy check.
- When the user mentions a new dietary constraint that comes from a prenatal appointment.
