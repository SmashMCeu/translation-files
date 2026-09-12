---
name: translate-new-keys
description: Use when a translation key added to a Minecraft `.lang` file is missing from other locales, e.g. a new key in `en.lang` not present in de/ru/lc. Detects newly added keys from the git diff and translates them into every existing locale that lacks them.
---

# Translate new keys

Propagate **newly added** keys from `en.lang` into sibling locales that lack them. Apply the format/encoding rules from `AGENTS.md`. Work only from the diff — never sweep for pre-existing missing keys.

## 1. Find newly added keys

```bash
git diff HEAD -- '*.lang'
```

Added lines start with `+` and look like `key=value`. Keep only keys added under an `en.lang`; ignore changes to other locales. Skip `+` lines that merely edit an existing key's value — flag those instead of propagating them.

## 2. Find which locales lack each key

For each new key, open the sibling `<locale>.lang` files in the same module (`de`, `ru`, `lc`, whichever exist) and check whether that exact key is present. A locale is missing the key iff it is absent.

## 3. Add each missing translation

Insert `key=translated` at the **same position as in `en.lang`** (mirror its neighbors); never append at the end. Use the `edit` tool anchored on the neighbors; change nothing else.

- Match the register/voice of neighboring keys (e.g. German chat uses `du`), not literal word-for-word.
- Copy `§` codes, `%s`/`%1$s`/`%2$s`/`%3$s` placeholders, and `<br>` verbatim; translate only the surrounding text.
- `de` → German, `ru` → Russian.
- `lc` → English in the existing lc style (e.g. `you`→`u`). Never fix its deliberate misspellings.

## 4. Verify

Re-run step 1; confirm each new key is present in every locale of its module at `en.lang`'s position.
