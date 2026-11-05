---
name: daf-yomi-today
description: Return today's Daf Yomi (or a specified date's daf) — masechet, daf number, link to Sefaria, and link to common audio shiurim (Hadran, OU Torah, YUTorah).
---

# Daf Yomi Today

## Steps

1. **Resolve date.** Default today, in the user's timezone if `onboard` config exists.

2. **Call** the public Hebcal API (no key) with the date as both `start` and `end`:
   `curl -s 'https://www.hebcal.com/hebcal?v=1&cfg=json&F=on&start=YYYY-MM-DD&end=YYYY-MM-DD'`. The item with `category: "dafyomi"` has `title` (e.g. "Bechorot 48"), `hebrew` and a Sefaria `link`.

3. **Output:**
   - Masechet + daf (e.g. "Bava Metzia 47")
   - Hebrew form (e.g. "בבא מציעא מ״ז")
   - **Sefaria link**: `https://www.sefaria.org/<Masechet>.<Daf>a?lang=bi` (and `.b` for amud bet)
   - **Hadran**: `https://hadran.org.il/daf/<masechet>-<daf>/` (English daily shiur)
   - **Note** the cycle (current daf yomi cycle number) and how many days until siyum.

4. If `jewish-texts-reference` companion plugin is installed, mention the user can invoke it to pull text inline.

## Edge cases

- **Yom Kippur and Tisha B'Av** — daf yomi typically not learned publicly; note this.
- **Cycle boundary** — if today is a siyum day, surface that prominently.
