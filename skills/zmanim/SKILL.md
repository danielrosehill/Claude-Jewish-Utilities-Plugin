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

3. **Call the zmanim MCP server.** Preferred backend is `zmanim-mcp-server` (https://github.com/ariroffe72/zmanim-mcp-server) — it uses python-zmanim (a port of KosherJava) with NOAA-based astronomical calculations and supports GR"A and MG"A opinions.

   Tools to use:
   - `zmanim_get_daily_times` — comprehensive daily set (preferred default)
   - `zmanim_get_sunrise_sunset`
   - `zmanim_get_shema_times` — GR"A and MG"A
   - `zmanim_get_tefila_times` — GR"A and MG"A
   - `zmanim_get_mincha_times` — Mincha Gedola, Mincha Ketana, Plag HaMincha
   - `zmanim_get_shabbat_times` — candle-lighting, havdalah

   Pass `latitude`, `longitude`, `timezone` from config. For shabbat times, pass `candle_lighting_offset` from config (default 18, Jerusalem 40).

   **If the zmanim MCP server isn't installed**, tell the user how to add it:

   ```bash
   claude mcp add zmanim -- uvx zmanim-mcp-server
   ```

4. **Fallback** — if the zmanim MCP isn't available and the user doesn't want to install it, use the hebcal REST endpoint: `https://www.hebcal.com/zmanim?cfg=json&latitude=...&longitude=...&tzid=...&date=YYYY-MM-DD`. Do **not** silently approximate.

5. **Format the response** as a table with timezone shown next to times. Group by category (dawn, morning prayer, midday, afternoon, sunset/night). Show GR"A and MG"A side-by-side where both apply.

## Optional flags

- "Use 72-minute opinion only" — filter the output.
- "Compare cities" — accept ad-hoc lat/lon for a one-off comparison without overwriting saved config.
- "Output JSON" — request the MCP's JSON format for further processing.
