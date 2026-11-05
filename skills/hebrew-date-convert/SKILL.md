---
name: hebrew-date-convert
description: Convert a date between Gregorian and Hebrew calendars in either direction. Handles before/after sunset (Hebrew dates roll over at sunset, not midnight) when location is configured.
---

# Hebrew Date Convert

## Direction

Detect direction from input:
- "What's 5th Iyar 5784 in Gregorian?" → Hebrew → Gregorian
- "What's the Hebrew date for 14 May 1948?" → Gregorian → Hebrew

## Steps

1. **Parse the input date.** Accept:
   - Gregorian: `YYYY-MM-DD`, `DD Month YYYY`, `Month DD, YYYY`.
   - Hebrew: `<day> <month-name> <year>` (e.g. "5 Iyar 5784"), or numeric form.

2. **Sunset awareness.** Hebrew calendar dates start at sunset, not midnight. If the user provides a Gregorian datetime (with time), and `onboard` config exists with location:
   - Compute sunset for the date at that location (use the `zmanim` skill backend).
   - If the time is after sunset, return the *next* Hebrew day.
   - Note the sunset rule clearly in the output.

   If no time is given, assume daytime (Hebrew date = same calendar day).

3. **Convert.** Preferred backend: the public Hebcal converter API (no key):
   - Gregorian to Hebrew: `curl -s 'https://www.hebcal.com/converter?cfg=json&date=YYYY-MM-DD&g2h=1&strict=1'` (add `gs=on` if after sunset). Returns `hy`, `hm`, `hd`, `hebrew` and `events`.
   - Hebrew to Gregorian: `curl -s 'https://www.hebcal.com/converter?cfg=json&hy=5787&hm=Kislev&hd=1&h2g=1'`. Returns `gy`, `gm`, `gd`.

   **Offline fallback:** if no MCP / network, use the `pyluach` Python library (https://github.com/simlist/pyluach):

   ```python
   from pyluach import dates
   g = dates.GregorianDate(2024, 5, 13)
   h = g.to_heb()  # HebrewDate
   ```

   `pyluach` handles full Hebrew calendar arithmetic (leap years, Adar I/II, weekday math).

4. **Output** both forms plus:
   - Hebrew date in Hebrew script (e.g. ה׳ אייר תשפ״ד) and transliteration.
   - Day of week.
   - Any holiday/fast/shabbat falling on that day.

## Edge cases

- **Adar I vs. Adar II** in leap years — be explicit.
- **Pre-1752 Gregorian dates** in some locales used Julian; flag if the user is doing genealogy.
- **JS context** — if the user is building a JS/TS app and asks for a library to do this client-side, recommend `jewish-date` (https://github.com/Shmulik-Kravitz/jewish-date).
