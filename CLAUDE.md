# Casinha Frosted

Apple-style frosted glass theme for Home Assistant. GitHub fork of
[wessamlauf/homeassistant-frosted-glass-themes](https://github.com/wessamlauf/homeassistant-frosted-glass-themes)
(MIT, credited in the README). Origin: `diniscorreia/casinha-frosted`. Remote `upstream` is the original.

## Layout

- `themes/Frosted Glass.yaml` and `themes/Frosted Glass Lite.yaml` are the **only hand-edited sources**. Each holds a light and a dark mode.
- `themes/Frosted Glass {Light,Dark,Light Lite,Dark Lite}.yaml` are **generated** by `python3 scripts/sync_themes.py` (CI runs it with `--check`). Never hand-edit them.
- Filenames keep the upstream names on purpose so upstream merges stay clean. Only the theme names (the YAML keys) are ours; the mapping is `THEME_NAMES` in `scripts/sync_themes.py`.
- Theme names: `Casinha Frosted`, `Casinha Frosted Lite`, plus `... Light`, `... Dark`, `... Light Lite`, `... Dark Lite`. Names must never clash with the original.
- Theme YAML uses CRLF line endings. Preserve them (edit with tooling that does, or run `sync_themes.py`).
- `reference/casinha_glass.yaml` is my other, standalone theme ("Casinha Glass"), kept only as a reference for fonts, sizes and Apple colours. It is not part of this theme and is not shipped.
- Checks: `python3 scripts/validate_themes.py` (needs `pyyaml` and `jinja2`). The `.cjs` scripts are the upstream browser tests (`npm ci` first).

## Conventions

- **Edit in place.** Change the existing source files; do not create copies or parallel files.
- **Font:** every font token uses
  `-apple-system, BlinkMacSystemFont, system-ui, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`.
  Only the code font stays monospace.
- **iOS text sizes:** heading card title 17px / 600; tile primary 15px / 600 / letter-spacing -0.2px; tile secondary 13px / letter-spacing -0.1px; `ha-font-size-xs` 11px; `ha-badge-font-size` 13px.
- **Colours (Apple system colours):**
  - Light: primary/accent/info `#007AFF`, success `#34C759`, warning `#FF9500`, error `#FF3B30`, text `#1D1D1F`, secondary `#6E6E73`, disabled `#AEAEB2`. Home on-colours: `#FFCC00` lights/switches, `#30B0C7` fan, `#5856D6` cover, `#AF52DE` media.
  - Dark: `#0A84FF`, `#30D158`, `#FF9F0A`, `#FF453A`, text `#F5F5F7`, secondary `#98989D`, disabled `#636366`. On-colours: `#FFD60A`, `#40C8E0`, `#5E5CE6`, `#BF5AF2`.
  - Climate and scene state colours follow `reference/casinha_glass.yaml`.
- **Shape:** cards 20px. Tile cards (`hui-tile-card`, ~56px tall) are full pills at 28px, set by a rule in `card-mod-card` (the blur layer inherits the radius). Dialogs 28px. Badges stay pills.
- **Glass rule:** the card **rim must always be much more opaque than the card fill**, or the glass look disappears. Applies to every variant (Full and Lite, light and dark). The validator enforces rim alpha >= 2x fill alpha for Bubble surfaces and requires the card rim to exceed the card fill. Keep it true when changing any fill or border alpha.
- Background is an image (`themes/frosted-glass-*-background.jpg`) served through jsDelivr from this repo's `main` branch.

## Syncing upstream

```bash
git fetch upstream && git merge upstream/main
```

Resolve conflicts in favour of our font, colour, shape and name changes, while taking upstream's structural and engine fixes. For the four generated files, do not merge by hand: resolve the two source files, then run `python3 scripts/sync_themes.py`. Afterwards run `validate_themes.py` and re-check the glass rule. New upstream theme names, fonts or colours need the same Apple treatment.

## Context

- My Home Assistant runs on a Raspberry Pi 5 with HA-MCP. The Pi is low-powered, so Lite variants matter.
- **UIX** is installed instead of card-mod. It is a drop-in replacement, so the `card-mod-*` theme keys keep working.
- My partner Ricardo uses the default theme, so this theme only affects me.
- `~/Projects/home-assistant/` holds my Home Assistant repos; my HA config may join this one there later.
- Install in HA: HACS -> Custom repositories -> `https://github.com/diniscorreia/casinha-frosted`, category Theme.
