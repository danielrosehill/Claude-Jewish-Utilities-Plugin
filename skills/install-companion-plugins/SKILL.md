---
name: install-companion-plugins
description: Offer to install Claude Code plugins that complement jewish-utilities. The primary companion is `jewish-texts-reference` (Sefaria text lookups for Tanakh, Talmud, Mishnah, Halakha, plus nikkud add/strip). Use when the user asks about installing companions, or proactively after onboarding.
---

# Install Companion Plugins

## Companions

### jewish-texts-reference (recommended)

- **Repo**: https://github.com/danielrosehill/Jewish-Texts-Reference-Plugin
- **What it does**: Sefaria lookups through its public REST API for Tanakh, Talmud, Mishnah, Halakha, Kabbalah, commentary, dictionaries, topics. Plus nikkud (Hebrew vowelisation) add/strip skills via Dicta nakdan, removenikud APIs, and offline regex fallbacks.
- **Why install alongside jewish-utilities**: skills here (parsha-this-week, daf-yomi-today) link to Sefaria; the companion plugin lets Claude pull the *full text* of those references inline rather than just linking out.
- **Install command**:

  ```bash
  claude plugins install jewish-texts-reference@danielrosehill
  ```

  (If the `danielrosehill` marketplace isn't yet added on this machine: `claude plugins marketplace add danielrosehill/Claude-Code-Plugins` first, then install.)

## Steps

1. Show the user the list above.
2. Ask which they want to install (default: jewish-texts-reference).
3. Run the install command via Bash.
4. Tell the user to **restart Claude Code** so the new plugin is loaded.
