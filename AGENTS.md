# AGENTS.md

Localization files for the SmashMC Minecraft network. This repo is plain text: there is **no build, test, or lint
tooling** (no package manager, Gradle, etc.). Do not go looking for a "build command".

## Layout

- One directory per plugin/module (`lobby/`, `smash/`, `global/`, ...), one `<locale>.lang` file per language.
- `en.lang` is the source of truth. Other locales frequently lag behind it (many missing keys) and may contain
  stale/extra keys. Add new keys to `en.lang` first; translating the other locales is optional (the
  `translate-new-keys` skill propagates newly added keys from the git diff). Don't delete another locale's key just
  because it is missing from `en`.
- Some modules ship only `en`/`de`: `build`, `quest`, `sekai`, `smashclassic`. `smashskin` has no `lc`; `snowballgame`
  has no `ru`.
- `lc` is an intentional leetspeak/joke variant. Never "correct" its deliberate misspellings.

## File format

- Java `.properties`-style: `key=value`, case-sensitive keys, `#` comment lines, blank lines between groups. Keys and
  group comments stay in English even in translated files.
- Never translate, remove, reorder, or re-count these tokens inside a value:
    - `§` + char — Minecraft color/format codes
    - `%s`, `%1$s`, `%2$s`, `%3$s` — format placeholders
    - `<br>` — line break
- Keys must be unique per file; duplicates silently override (last wins). Existing duplicates: `build/{en,de}.lang` (
  `item.positions.lore`) and `giveaway/en.lang` (`chat.giveaway.already_qualified`). Don't add more.
- When adding a missing key to a locale, insert it at the same position as in `en.lang` (mirror its neighbors); never
  append at the end.
- Encoding is UTF-8 without BOM. The working tree is CRLF while the git index stores LF (`core.autocrlf=true`, no
  `.gitattributes`). Do not normalize line endings, reformat files, or add `.gitattributes` — it would produce an
  enormous diff.

## Commits & deploy

- Commit messages use a gitmoji prefix + short description, e.g. `:sparkles: Add chat.emoticon.header`,
  `:bug: Fix team warning colors`, `:pencil2: Fix wrong color code`.
- `.woodpecker.yml` copies the entire repo to the production server on every push to `main`. Do not push unless
  explicitly asked.
