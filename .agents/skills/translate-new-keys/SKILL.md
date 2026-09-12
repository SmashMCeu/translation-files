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

Added lines start with `+` and look like `key=value`. Keep only keys added under an `en.lang`; ignore changes to other locales. A `+` line may also be an in-place edit to an **existing** key's value — if that key already existed in `en.lang` before the diff, it is not a new key; leave other locales alone and flag the edit to the user instead of propagating it.

## 2. Find which locales lack each key

For each new key, open the sibling `<locale>.lang` files in the same module (`de`, `ru`, `lc`, whichever exist) and check whether that exact key is present. A locale is missing the key iff it is absent.

## 3. Add each missing translation

Insert `key=translated` at the **same position as in `en.lang`** — mirror its neighbors, never append at the end of the file. Use the `edit` tool, anchoring on the neighboring key(s); change nothing else.

- Find the key's place in `en.lang`: insert directly after the same preceding key (or before the same following key) in the target locale, so group ordering matches `en.lang`.
- If the target locale lacks the exact neighbor, anchor on the closest key that does exist; keep the new key inside the same group.
- Match the register/voice of the surrounding keys (e.g. German chat messages address the player with `du`), not a literal word-for-word rendering.
- Copy `§` codes, `%s`/`%1$s`/`%2$s`/`%3$s` placeholders, and `<br>` verbatim; translate only the surrounding text.
- `de` → German, `ru` → Russian.
- `lc` → English words in the existing lc style (leetspeak/broken spelling, e.g. `you`→`u`). Never fix its deliberate misspellings.

## 4. Verify

Re-run step 1 and confirm each new key now also appears in every locale file of its module, at the same relative position as in `en.lang`.
