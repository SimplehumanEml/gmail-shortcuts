# Gmail keyboard shortcuts, as open data

A machine-readable map of **every Gmail keyboard shortcut** plus the extra
shortcuts added by [CMDK](https://cmdk.email), the Gmail productivity
extension. One JSON file, a documented schema, and a permissive license, so
you can build on it.

- **[`gmail-shortcuts.json`](./gmail-shortcuts.json)** — the dataset (95 shortcuts).
- **[`gmail-shortcuts.schema.json`](./gmail-shortcuts.schema.json)** — its JSON Schema (draft 2020-12).
- **[`gmail-shortcuts.jsonl`](./gmail-shortcuts.jsonl)** — the same data flattened to one shortcut per line, keys split into columns (for spreadsheets and dataset viewers).

## Why this exists

Gmail's shortcut reference is spread across a help page, an in-app overlay, and
tribal knowledge, and none of it is machine-readable. This repo gives you a
single structured source you can drop into a cheat sheet, a docs site, an
onboarding flow, or a model's context window.

Each entry is tagged with its `origin`:

- `gmail-native` — a built-in Gmail shortcut (requires "Keyboard shortcuts on"
  in Gmail settings).
- `cmdk` — added by the CMDK extension.

So you can use the native set on its own, or show both.

## Schema

```json
{
  "version": "1.0.0",
  "generated": "2026-08-17",
  "license": "CC-BY-4.0",
  "counts": { "total": 95, "gmailNative": 59, "cmdk": 36 },
  "shortcuts": [ ... ]
}
```

Each shortcut:

| Field | Type | Notes |
| --- | --- | --- |
| `id` | string | Stable, unique identifier. |
| `keys` | object | Either `{ "default": "e" }` or, for shortcuts that use the platform modifier, `{ "mac": "Cmd + K", "windows": "Ctrl + K" }`. |
| `action` | string | Short name of what it does. |
| `description` | string | One-line explanation. |
| `category` | string | Slug, e.g. `navigation`, `actions`, `compose`. |
| `categoryLabel` | string | Human-readable category. |
| `origin` | enum | `gmail-native` or `cmdk`. |
| `requiresSetting` | string | Present on native rows: the Gmail setting they need. |
| `requiresExtension` | boolean | Present and `true` on CMDK rows. |

## Use it

```js
const data = await fetch(
  "https://raw.githubusercontent.com/SimplehumanEml/gmail-shortcuts/main/gmail-shortcuts.json"
).then((r) => r.json());

// Native Gmail shortcuts only
const native = data.shortcuts.filter((s) => s.origin === "gmail-native");

// Look up how to archive
const archive = data.shortcuts.find((s) => s.action === "Archive");
console.log(archive.keys.default); // "e"
```

```python
import json, urllib.request

url = "https://raw.githubusercontent.com/SimplehumanEml/gmail-shortcuts/main/gmail-shortcuts.json"
data = json.load(urllib.request.urlopen(url))
compose = [s for s in data["shortcuts"] if s["category"] == "compose"]
```

## License and attribution

Licensed under [Creative Commons Attribution 4.0 International](./LICENSE)
(CC BY 4.0). You are free to use, adapt, and redistribute this data, including
commercially, as long as you give credit.

**Attribution:** "Gmail shortcut data by [CMDK](https://cmdk.email), CC BY 4.0."
A link back to <https://cmdk.email> satisfies the requirement.

## Keeping it accurate

The dataset is generated from source, not hand-maintained, so it tracks the
real product. Corrections and additions are welcome by issue or pull request.
If Gmail changes a shortcut, open an issue and we will regenerate.
