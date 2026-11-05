---
name: shabbat-times
description: Get candle-lighting and havdalah times for the upcoming (or a specified) shabbat. Uses the location saved by the `onboard` skill. Calls the public Hebcal REST API (no key).
---

# Shabbat Times

Return candle-lighting and havdalah times for the user's saved location.

## Steps

1. **Read config:**

   ```bash
   CONFIG="${CLAUDE_USER_DATA:-${XDG_DATA_HOME:-$HOME/.local/share}/claude-plugins}/jewish-utilities/config.json"
   ```

   If missing, tell the user to run `/jewish-utilities:onboard` first and stop.

2. **Resolve target shabbat.** Default to the upcoming one. If the user specifies a date, use that week.

3. **Call the Hebcal REST API** (public, no key):

   ```bash
   curl -s 'https://www.hebcal.com/shabbat?cfg=json&M=on&latitude=LAT&longitude=LON&tzid=TZ&b=OFFSET&dt=YYYY-MM-DD'
   ```

   Use `geonameid=<id>` instead of lat/lon/tzid where known (Jerusalem is 281184). `b` is the candle-lighting offset in minutes from config (omit for Hebcal's default of 18). `dt` is optional and selects another week. `items` holds `category: "candles"`, `"parashat"` and `"havdalah"` entries.

4. **Get the parsha** from the same response (`category: "parashat"`) and include it in the output.

5. **Format the response.** Show:
   - Parsha name (Hebrew + transliteration)
   - Candle-lighting time (local, with timezone)
   - Havdalah time (local) — note method used (tzeit 3 stars, fixed minutes, etc.)
   - Any holiday overlap (e.g. "Erev Pesach")

## Edge cases

- **Two-day yom tov adjacent to shabbat** — call out that candle-lighting and havdalah may shift; show all relevant times.
- **Above the polar circle / extreme latitudes** — surface backend errors verbatim; do not guess.
- **No internet / MCP unavailable** — report the error; do not fall back to local approximation.
