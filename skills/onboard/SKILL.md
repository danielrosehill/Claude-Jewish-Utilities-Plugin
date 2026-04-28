---
name: onboard
description: First-run setup for the jewish-utilities plugin. Captures the user's location (city, latitude, longitude, timezone) and writes it to a config file so that location-dependent skills (shabbat-times, zmanim) can read it without re-asking each time. Run this once before using shabbat-times or zmanim, or any time the user moves.
---

# Onboard

This skill establishes per-user context for the jewish-utilities plugin. Most importantly, it captures the user's **geographic location**, which is required for any halachic time calculation (shabbat candle-lighting, havdalah, zmanim).

## Storage location

Resolve the data root with this snippet (POSIX shell):

```bash
DATA_ROOT="${CLAUDE_USER_DATA:-${XDG_DATA_HOME:-$HOME/.local/share}/claude-plugins}/jewish-utilities"
mkdir -p "$DATA_ROOT"
CONFIG_FILE="$DATA_ROOT/config.json"
```

Never write to `~/.claude/plugins/jewish-utilities/` — that directory is clobbered on plugin update.

## What to ask

Interview the user for:

1. **City** (free text, e.g. "Jerusalem", "Brooklyn, NY"). Used as a human-readable label and as a lookup key for hebcal where supported.
2. **Latitude / longitude** (decimal degrees). Required for precise zmanim. If the user doesn't know, geocode the city and confirm the result with the user before saving.
3. **Timezone** (IANA name, e.g. `Asia/Jerusalem`, `America/New_York`). If unsure, derive from the lat/lon and confirm.
4. **Candle-lighting offset** (minutes before sunset; default 18, Jerusalem custom 40). Optional — leave null to use hebcal's default.
5. **Havdalah method** (e.g. `tzeit_3_stars`, fixed minutes after sunset). Optional.
6. **Nusach** (Ashkenaz / Sephard / Edot HaMizrach) — optional, used by some text-rendering tools.

## What to save

Write `config.json`:

```json
{
  "version": 1,
  "location": {
    "city": "Jerusalem",
    "latitude": 31.7683,
    "longitude": 35.2137,
    "timezone": "Asia/Jerusalem"
  },
  "candle_lighting_minutes": 40,
  "havdalah_method": "tzeit_3_stars",
  "nusach": "ashkenaz"
}
```

If a config already exists, read it first, show the user current values, and only overwrite fields they want to change.

## After saving

Tell the user:

- Where the config was saved.
- That `shabbat-times` and `zmanim` will now use this location automatically.
- That they can re-run `/jewish-utilities:onboard` any time to update.
- Suggest they also run `/jewish-utilities:install-companion-plugins` to install `jewish-texts-reference` for Sefaria text lookups.
