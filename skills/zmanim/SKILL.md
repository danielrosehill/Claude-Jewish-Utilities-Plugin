---
name: zmanim
description: Return halachic times (zmanim) for the user's saved location for today or a specified date — alot, sunrise, latest shema, latest tefillah, chatzot, mincha gedolah/ketana, plag, sunset, tzeit hakochavim, candle-lighting, havdalah. Supports GR"A and MG"A opinions. Requires onboarding first.
---

# Zmanim

Return halachic times for a given date and the user's saved location.

## Steps

1. **Read config:**

   ```bash
   CONFIG="${CLAUDE_USER_DATA:-${XDG_DATA_HOME:-$HOME/.local/share}/claude-plugins}/jewish-utilities/config.json"
   ```

   If missing, prompt the user to run `/jewish-utilities:onboard` first and stop.

2. **Resolve target date.** Default to today (in the user's timezone). Accept Gregorian or Hebrew date input.

3. **Call the public Hebcal zmanim API** (no key, no MCP):

   ```bash
   curl -s 'https://www.hebcal.com/zmanim?cfg=json&geonameid=281184&date=YYYY-MM-DD'
   ```

   Use `geonameid` where known (Jerusalem is 281184), otherwise `latitude`, `longitude` and `tzid` from config. `times` holds GR"A and
   MG"A values: `alotHaShachar`, `sunrise`, `sofZmanShma`, `sofZmanShmaMGA`, `sofZmanTfilla`, `sofZmanTfillaMGA`, `chatzot`,
   `minchaGedola`, `minchaKetana`, `plagHaMincha`, `sunset` and more. For candle lighting and havdalah use `shabbat-times`.

4. **If the API fails,** report the error. Do **not** approximate times.

5. **Format the response** as a table with timezone shown next to times. Group by category (dawn, morning prayer, midday, afternoon, sunset/night). Show GR"A and MG"A side-by-side where both apply.

## Optional flags

- "Use 72-minute opinion only" — filter the output.
- "Compare cities" — accept ad-hoc lat/lon for a one-off comparison without overwriting saved config.
- "Output JSON" — return the API's `times` object for further processing.
