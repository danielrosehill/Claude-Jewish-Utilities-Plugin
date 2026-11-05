---
name: upcoming-jewish-holidays
description: List upcoming Jewish holidays — major yom tov, minor holidays, fasts, and rosh chodesh — for the next N days, the rest of the current Hebrew year, or a specified Gregorian/Hebrew year. Diaspora vs. Israel observance is honored based on the user's saved location.
---

# Upcoming Jewish Holidays

## Steps

1. **Resolve scope.** Default: next 90 days. Accept:
   - "next 30 days"
   - "rest of 5785"
   - "in 2026"
   - "all of 5786"

2. **Resolve `il` flag.** Read `onboard` config. If `location.city` is in Israel (or user explicitly sets), pass `il=true` to hebcal — affects second-day yom tov.

3. **Call** the public Hebcal API (no key), with `start`/`end` set to the span:
   `curl -s 'https://www.hebcal.com/hebcal?v=1&cfg=json&maj=on&min=on&mod=on&nx=on&mf=on&ss=on&start=YYYY-MM-DD&end=YYYY-MM-DD'` and add `i=on` for Israel. Each item has `title`, `date`, `hdate`, `hebrew` and `category`. Chain calls if the span is over a year.

4. **Filter** by category if requested:
   - Major (yom tov): Pesach, Shavuot, Rosh Hashanah, Yom Kippur, Sukkot, Shemini Atzeret/Simchat Torah
   - Minor: Chanukah, Purim, Tu B'Shvat, Lag B'Omer, Tu B'Av
   - Fasts: Tzom Gedaliah, 10 Tevet, Ta'anit Esther, 17 Tammuz, 9 Av
   - Rosh Chodesh
   - Modern: Yom HaShoah, Yom HaZikaron, Yom HaAtzmaut, Yom Yerushalayim

5. **Output** as a table:
   - Holiday name (Hebrew + transliteration)
   - Hebrew date
   - Gregorian date
   - Day of week
   - Notes (work-prohibited? fast? candle-lighting required?)

## Optional outputs

- ICS export — if the user asks for calendar import, write an `.ics` file under `$CLAUDE_USER_DATA/jewish-utilities/data/holidays-<year>.ics`.
