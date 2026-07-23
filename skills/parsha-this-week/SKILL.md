---
name: parsha-this-week
description: Return this week's Torah portion (parsha) — name in Hebrew and transliteration, verse range, summary, and a link to the full text on Sefaria. Optionally accepts a date to look up a different week.
---

# Parsha This Week

## Steps

1. Default to the current week (use the user's timezone if available from `onboard` config; otherwise system time).

2. Call `mcp__gateway__hebcal__torah-portion` to get the parsha name and verse range. In the diaspora vs. Israel, parshiyot can differ — pass the user's location or `il=true/false` based on saved config.

3. Output:
   - **Parsha name** (Hebrew + transliteration)
   - **Verse range** (e.g. Bereshit 1:1–6:8)
   - **Brief summary** (2–4 sentences) — synthesise from general knowledge.
   - **Sefaria link** — `https://www.sefaria.org/<Book>.<chapter>.<verse>?lang=bi`

4. If the user has the `jewish-texts-reference` companion plugin installed, suggest they invoke it for full text.

## Edge cases

- **Double parshiyot** (e.g. Tazria-Metzora) — return both names.
- **Holiday weeks** (e.g. Pesach, Sukkot) — there's a special reading instead of the regular cycle; surface that.
- **Israel vs. diaspora drift** — happens when a yom tov falls on shabbat in the diaspora; flag clearly.
