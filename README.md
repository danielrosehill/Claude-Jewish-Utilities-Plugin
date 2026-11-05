# Jewish-Utilities

Miscellaneous utility skills for Jewish users — shabbat times, zmanim, parsha-of-the-week, hebrew date conversion, upcoming holidays, and daf yomi. Calls the public [Hebcal REST API](https://www.hebcal.com/home/developer-apis) (no key, no MCP) and complements [`jewish-texts-reference`](https://github.com/danielrosehill/Jewish-Texts-Reference-Plugin) for full-text Sefaria lookups.

## Skills

| Skill | What it does |
|---|---|
| `onboard` | Capture city / lat-lon / timezone / candle-lighting and havdalah preferences. Run this first. |
| `shabbat-times` | Candle-lighting and havdalah for the upcoming (or specified) shabbat at the saved location. |
| `zmanim` | Daily halachic times (alot, sunrise, shema, chatzot, mincha, plag, sunset, tzeit) for a date and the saved location. |
| `parsha-this-week` | Current weekly Torah portion with Sefaria link. |
| `hebrew-date-convert` | Convert dates between Gregorian and Hebrew calendars (sunset-aware). |
| `upcoming-jewish-holidays` | List upcoming yom tov, minor holidays, fasts, rosh chodesh; honors Israel/diaspora observance. |
| `daf-yomi-today` | Today's daf with Sefaria + shiur links. |
| `install-companion-plugins` | Install `jewish-texts-reference` for full-text Sefaria lookups. |

## Installation

```bash
claude plugins marketplace add danielrosehill/Claude-Code-Plugins
claude plugins install jewish-utilities@danielrosehill
```

After install, restart Claude Code, then run:

```
/jewish-utilities:onboard
```

to set your location (required for `shabbat-times` and `zmanim`).

## Dependencies

- **[zmanim-mcp-server](https://github.com/ariroffe72/zmanim-mcp-server)** (recommended for `zmanim` and `shabbat-times`) — Python MCP using python-zmanim / KosherJava with NOAA-based astronomical calculations and GR"A + MG"A opinion support.

  ```bash
  claude mcp add zmanim -- uvx zmanim-mcp-server
  ```

- **Hebcal REST API** — calendar, holiday, parsha, daf-yomi and date-conversion endpoints (https://www.hebcal.com/home/developer-apis). Public, no key; the skills call it with `curl`.
- **`jewish-texts-reference` plugin** (optional, recommended) — inline Tanakh/Talmud/Mishnah text via Sefaria, instead of link-outs.

## Related tools & references

These aren't required dependencies — they're tools the skills reference, recommend, or could be extended with:

- [hebcal/hebcal](https://github.com/hebcal/hebcal) — the upstream open-source Hebrew calendar engine that backs the Hebcal API.
- [simlist/pyluach](https://github.com/simlist/pyluach) — Python Hebrew-calendar arithmetic; used as offline fallback in `hebrew-date-convert`.
- [Shmulik-Kravitz/jewish-date](https://github.com/Shmulik-Kravitz/jewish-date) — TypeScript Hebrew-date library; recommended when the user is building a JS/TS app.
- [torahcalc/torahcalc](https://github.com/torahcalc/torahcalc) — Torah calculator (gematria, biblical units, dates). Useful reference; could be wrapped in a future `gematria` skill.
- [abe-101/myHebrewDates](https://github.com/abe-101/myHebrewDates) — Django app for tracking yahrzeits and Hebrew-date birthdays/anniversaries. Reference for users who want a hosted reminder service.
- [subalterngames/talmudifier-rs](https://github.com/subalterngames/talmudifier-rs) — Rust tool that formats text in classic Talmud-page layout (with commentary columns). Relevant if rendering study sheets.

## Data storage

Per-user config (location, candle-lighting offset, havdalah method, nusach) is stored at:

```
${CLAUDE_USER_DATA:-${XDG_DATA_HOME:-$HOME/.local/share}/claude-plugins}/jewish-utilities/config.json
```

Nothing is written to `~/.claude/plugins/jewish-utilities/` (that directory is replaced on plugin update).

## License

MIT
