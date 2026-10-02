# Localization

Choose the interface language in **Settings > General > Language**. Bundled language codes are `en`, `ru`, `es`, `ja`, `ko`, `zh_cn`, and `zh_tw`. Missing translated text falls back to English.

## External language files

G3M reads `lang/lang_<code>.json` under its data directory. Start with a copy of the English file so you have the actual keys and formatting placeholders.

For a separate translation, use a distinct code such as `lang_example.json`. Its `metadata` can specify `language_name`, `qt_translation`, and `font`. A font path is relative to the language file's folder.

Example fragment showing the structure:

```json
{
  "metadata": {
    "language_name": "Example language",
    "qt_translation": "qtbase_en"
  },
  "common": {
    "close": "Close"
  }
}
```

Preserve substitutions such as `{game}`, `{count}`, and `{error}` exactly. Translate the surrounding text. Keep required HTML markup valid where a value uses it. Save UTF-8 JSON and restart G3M to select the external language.

## Customize a bundled translation

Bundled keys are synchronized into the external files. To keep an intentional text override, replace its leaf key with an underscore-prefixed key rather than leaving both keys present:

```json
{
  "common": {
    "_close": "Dismiss"
  }
}
```

This is a fragment inside the full external language file. The ordinary `close` key must be absent in that object for `_close` to be used. Metadata for bundled language files is synchronized from the bundled version; use a separate language file for a different name or font.

Plugins provide their own language files through the [plugin API](../features/plugins-development.md). They use their plugin's translation namespace rather than modifying unrelated application keys.
