# i18n Skills for Claude Code

Slash commands that turn Claude Code into an i18n expert — audit your existing translations for quality issues, or wrap hardcoded strings with the correct i18n function for your framework. Works with any codebase: React, Vue, Angular, Svelte, Python, Ruby, Flutter, iOS, Android.

## Install

```bash
cp -r commands/*.md ~/.claude/commands/
```

Then restart Claude Code. The commands are now available in any project.

## Commands

### `/i18n-review` — Audit existing i18n wrapping

Read-only audit that finds quality issues in your existing translations:

- **Split sentences** — `t('hello') + name` breaks word order in Japanese, Arabic, etc.
- **Over-wrapping** — `t('PDF')`, `t('API')` — technical constants that don't need translation
- **Under-wrapping** — `"Cancel"`, `"Email"` — user-facing strings missing `t()` calls
- **Broken plurals** — `count === 1 ? 'item' : 'items'` instead of proper plural rules
- **Unused keys** — translation keys with no code references (saves translator cost)
- **Hardcoded values** — `"14 days"` baked into translations instead of `{count}` parameters

```
> /i18n-review src/components/

i18n Review: src/components/
========================================
Framework: next-intl | Source: en | Targets: [de, ja, es, fr]

=== 🔴 CRITICAL (3) ===
  src/components/Pricing.tsx:42 -- t('price') + '$' + amount -- Fix: single key t('price', {amount}) with Intl.NumberFormat
  src/components/Header.tsx:18 -- t('welcome') + userName -- Fix: t('welcome', {name})
  src/components/Cart.tsx:31 -- count === 1 ? 'item' : 'items' -- Fix: ICU plural {count, plural, one{# item} other{# items}}

=== 🟡 WARNING (5) ===
  src/components/Badge.tsx:12 -- t('api.label') wraps "API" -- technical acronym, unwrap
  src/components/Form.tsx:28 -- "Submit" not wrapped -- user-facing button label

=== SUMMARY ===
  🔴 Critical: 3  |  🟡 Warning: 5  |  🔵 Info: 2
```

### `/i18n-wrap` — Wrap hardcoded strings

Scans files, wraps hardcoded strings with the right i18n function, updates locale files, and validates the build:

```
> /i18n-wrap src/components/Dashboard.tsx

i18n Wrap: src/components/Dashboard.tsx
========================================
Framework: next-intl | Source: en | Targets: [de, ja, es, fr]

--- SCAN ---
  Files: 1 scanned | Strings: 8 found | Already wrapped: 2 | Newly wrapped: 6

--- KEYS ---
  dashboard.title: "Dashboard"
  dashboard.welcome: "Welcome back, {name}"
  dashboard.stats.items: "{count, plural, one{# item} other{# items}}"
  common.buttons.save: "Save changes"

--- LOCALE FILES ---
  messages/en.json: +6 keys
  Missing translations: 6 across 4 locales

--- VALIDATION ---
  Build: PASS | Lint: PASS | Tests: PASS (2 updated)
```

You can target a directory or specify a framework:

```bash
/i18n-wrap src/
/i18n-wrap src/ --framework=react-i18next
/i18n-review src/ --framework=vue-i18n
```

## Supported Frameworks

| Framework | Translation function | Plural format |
|-----------|---------------------|---------------|
| next-intl | `useTranslations(ns)` | ICU MessageFormat |
| react-i18next | `useTranslation(ns)` | Suffix keys (`_one`, `_other`) |
| vue-i18n | `$t()` / `useI18n()` | Pipe syntax |
| Angular | `$localize` / `i18n` attr | ICU in templates |
| Svelte | `$_('key')` | ICU MessageFormat |
| Python | `_("str")` / `gettext()` | `ngettext()` |
| Ruby | `t('key')` / `I18n.t()` | YAML `count:` |
| Flutter | `AppLocalizations.of(context)` | ICU in ARB |
| iOS | `NSLocalizedString` | `.stringsdict` |
| Android | `getString(R.string.key)` | `<plurals>` XML |

## License

MIT — see [LICENSE](LICENSE).

Built by [i18n Agent](https://i18nagent.ai)
