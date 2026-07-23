---
name: shabbat-times
description: Get candle-lighting and havdalah times for the upcoming (or a specified) shabbat. Uses the location saved by the `onboard` skill. Calls the zmanim MCP server (preferred) or hebcal MCP/REST as fallback.
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

3. **Call backend.** Preferred:
   - `zmanim_get_shabbat_times` from the zmanim MCP server (https://github.com/ariroffe72/zmanim-mcp-server). Pass lat/lon/timezone and `candle_lighting_offset` from config.

   Fallback:
   - `mcp__gateway__hebcal__shabbat-times` — hebcal MCP.
   - `https://www.hebcal.com/shabbat?cfg=json&latitude=...&longitude=...&tzid=...` REST endpoint.

4. **Get the parsha** via `mcp__gateway__hebcal__torah-portion` for the same week and include it in the output.

5. **Format the response.** Show:
   - Parsha name (Hebrew + transliteration)
   - Candle-lighting time (local, with timezone)
   - Havdalah time (local) — note method used (tzeit 3 stars, fixed minutes, etc.)
   - Any holiday overlap (e.g. "Erev Pesach")

## Edge cases

- **Two-day yom tov adjacent to shabbat** — call out that candle-lighting and havdalah may shift; show all relevant times.
- **Above the polar circle / extreme latitudes** — surface backend errors verbatim; do not guess.
- **No internet / MCP unavailable** — report the error; do not fall back to local approximation.
