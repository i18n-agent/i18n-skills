# i18n-skills

AI-powered slash commands for auditing and wrapping i18n strings in any codebase.

## What Are Skills?

Skills are reusable slash commands for AI coding assistants (Claude Code, etc.). Each skill is a structured prompt that turns the assistant into a specialized i18n agent with deep knowledge of framework-specific patterns, plural rules, and localization best practices.

## Available Commands

### `/i18n-review` -- Audit existing i18n wrapping

Read-only audit that scans your codebase for i18n quality issues:

- **Split sentences** -- concatenated `t()` calls that break word order in other languages
- **Over-wrapping** -- technical constants (API, PDF, USD) incorrectly wrapped with `t()`
- **Under-wrapping** -- user-facing strings missing `t()` calls
- **Plural issues** -- manual ternaries instead of framework plural support, missing plural categories
- **Unused keys** -- translation keys with no code references
- **Hardcoded values** -- dates, prices, counts baked into translation strings instead of using parameters

```bash
/i18n-review src/
/i18n-review src/ --framework=next-intl
```

### `/i18n-wrap` -- Wrap hardcoded strings with i18n functions

Scans source files for hardcoded user-facing strings and wraps them with the correct i18n function for your framework:

- Auto-detects your framework (next-intl, react-i18next, vue-i18n, Angular, Svelte, Python, Ruby, Flutter, iOS, Android)
- Generates semantic translation keys with proper namespacing
- Handles interpolation, plurals, and rich text markup
- Updates locale files with new keys
- Validates build, lint, and tests after changes

```bash
/i18n-wrap src/components/
/i18n-wrap src/ --framework=react-i18next
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
| iOS | `NSLocalizedString` / `String(localized:)` | `.stringsdict` |
| Android | `getString(R.string.key)` | `<plurals>` XML |

## Installation

Copy the `commands/` directory into your project's `.claude/commands/` folder:

```bash
cp -r commands/ /path/to/your-project/.claude/commands/
```

Or copy individual command files:

```bash
mkdir -p .claude/commands
cp commands/i18n-review.md .claude/commands/
cp commands/i18n-wrap.md .claude/commands/
```

Then use them as slash commands in Claude Code.

## License

MIT License - see [LICENSE](LICENSE) file.

Built by [i18nagent.ai](https://i18nagent.ai)
