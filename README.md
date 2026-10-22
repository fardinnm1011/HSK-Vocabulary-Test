<div align="center">

# HanziFlow 汉字流

**A free, offline-first Chinese trainer for HSK study.**
Vocabulary drills, sentence practice and translation — all running in your browser, with your data staying on your device.

[![License: Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
![Version](https://img.shields.io/badge/version-3.5.1-teal.svg)
![Offline first](https://img.shields.io/badge/offline-first-success.svg)
![No build step](https://img.shields.io/badge/build-none-lightgrey.svg)
[![Support on Ko-fi](https://img.shields.io/badge/Ko--fi-Support%20HanziFlow-ff5e5b?logo=ko-fi&logoColor=white)](https://ko-fi.com/fardinfahim63404)

</div>

---

## What is HanziFlow?

HanziFlow is a single-file web app (plain HTML, CSS and JavaScript — no account, backend, build process or installation). Import your own vocabulary spreadsheet, practise in several directions, write and translate sentences, and review what you keep getting wrong. Everything works offline, and your study data is stored locally in your browser.

> **Privacy by design:** your vocabulary, settings and progress never leave your device. The optional AI features are **off by default**; when you turn them on, only the single question being checked is sent to the provider you choose, using your own API key.

## What's new in v3

| Area | Highlights |
|---|---|
| **Three designs** | **Material 3 Expressive**, **Liquid Glass** (translucent layered panels, blur, animated glass selection indicator) and a **Nothing-inspired** dot-matrix look. |
| **English ⇄ 中文 interface** | One button in the top bar switches the whole interface between English and Chinese, with an animated transition. Your choice is remembered. |
| **Light / dark** | A dedicated light/dark button (animated circular reveal), separate from Settings. "Follow system" is still available. |
| **Sentence practice** | Write your own sentence with a target word, or translate sentences in four directions. 112 bundled sentences work fully offline. |
| **Optional AI checking** | Choose from six providers — see [AI providers](#optional-ai-checking). Fully optional; local checking always works. |
| **Safer data** | Full JSON backup & restore, automatic safety snapshots before risky actions, and upgrade from v2.0 data without touching the originals. |
| **Accessibility** | Reduced-motion aware, keyboard-friendly, proper focus outlines, and contrast-checked themes. |


## Features

**Practise**
- **Vocabulary exercises:** Chinese → English, English → Chinese, Pinyin practice, Pinyin → English, and Mixed practice.
- **Sentence creation:** write your own sentence with a target word.
- **Sentence translation:** four directions, mixed or one at a time.
- **Choose what to practise:** all words, specific HSK levels, or ticked words; sentences from bundled, imported, your own, or AI-generated sources.
- **Mistake review:** weak words come back more often; sentence corrections and AI judgements are kept for a second look.

**Flexible answer checking**
- Normalised English answers (case, spacing, articles, bracketed notes) with *normal*, *strict* and *lenient* matching.
- Hanzi validation, Pinyin with tone marks or tone numbers, strict or tone-insensitive checking, optional homophone acceptance.
- Configurable attempts per question, hints, skip and reveal. Empty or badly formatted answers never cost an attempt.
- **Accepted alternatives** you approve are stored separately from your original spreadsheet data.

**Your data**
- Import `.xlsx`, `.xls` or `.csv` (sheet picker, column mapping, preview, invalid-row report). Duplicates are handled sensibly, and polyphones (e.g. 行 *xíng* / *háng*) can be kept as separate entries.
- Starter set of 69 bundled words if you just want to try it.
- Export vocabulary (CSV) and statistics (CSV/JSON); full backup and restore (JSON).
- Statistics: accuracy overall, by exercise and by HSK level, plus practice history.

**Interface**
- Three designs, light/dark/system theme, adjustable motion (*System / Full / Reduced / Minimal*) and Hanzi style (Sans or Kai).
- English ⇄ 中文 interface switch.
- Fonts: Google Sans, Inter and Noto Sans are bundled. If you have **SF Pro Display** installed, the Liquid Glass design uses it (Apple fonts can't be redistributed, so it is not bundled).

> **HSK levels:** levels attached to the bundled starter words and sentences are author-assigned and are **not** an official HSK classification. For your own imported lists, HanziFlow only uses levels that are in your file — it never guesses.

## Getting started

### 1. Get the file

Download **`index.html`** (the single-file app) from this repository or the latest release. Nothing else is required.

### 2. Open it

**Option A — open directly.** Double-click `index.html`. Use a recent browser (see [Browser support](#browser-support)).

**Option B — use a local server** (recommended if you use the AI features, or if your browser restricts local-file features):

```bash
# in the folder that contains index.html
python -m http.server 8000      # Windows: py -m http.server 8000
```

Then open <http://localhost:8000>.

> **Important:** browser storage for `file://` and `http://localhost:8000` is **separate**. Pick one launch method and keep using it if you want to see the same saved progress.

**Linux:** if Chinese characters show as boxes, install CJK fonts, e.g. on Arch: `sudo pacman -S noto-fonts-cjk`.

### Browser support

HanziFlow's themes use CSS `light-dark()`, so you need **Chrome/Edge 123+, Firefox 120+ or Safari 17.5+**. Older browsers show an "update your browser" message.

## Importing vocabulary

On the **Dashboard** or **Vocabulary** page choose **Import vocabulary** (or drop a file onto the drop zone).

At minimum include columns for Chinese characters, Pinyin and English meaning. Optional columns: HSK level, framework, accepted answers, tags, notes.

| Chinese (Hanzi) | Pinyin   | English Meaning    | Level |
|-----------------|----------|--------------------|-------|
| 学习            | xuéxí    | to study; to learn | HSK 1 |
| 朋友            | péngyou  | friend             | HSK 1 |
| 重要            | zhòngyào | important          | HSK 2 |

Header recognition is flexible (`Chinese`, `Hanzi`, `汉字`, `拼音`, `Pinyin`, `Meaning`, `English`, `英文`, …). If detection isn't confident, use the column mapper. Your spreadsheet is treated as a *reference*: AI-approved extra meanings are saved separately and never overwrite it.

Progress belongs to a word's **Hanzi + Pinyin** identity. Editing only the English meaning keeps progress; changing Hanzi or Pinyin makes it a different word.

## Optional AI checking

AI is **optional**. Without it every answer is checked locally and nothing is sent anywhere. With it, HanziFlow can judge English meanings and sentences more flexibly, pick target words from your imported vocabulary, and generate extra practice sentences (clearly labelled *unverified*).

Open **Settings → Connectivity & AI**, pick a provider and add **your own API key**. AI stays off until a key is saved for the selected provider.

| Provider | Default model ID | Default endpoint |
|---|---|---|
| Google Gemini | `gemini-3.8-flash` | Google AI |
| Qwen 3.8 Flash | `qwen3.8-flash` | `dashscope.aliyuncs.com` (mainland China) |
| Kimi K3 | `kimi-k3` | `api.moonshot.ai` |
| DeepSeek V4.1 Flash (1017) | `deepseek-flash` | `api.deepseek.com` |
| GPT-6 Luna | `gpt-6-luna` | `api.openai.com` |
| Claude Haiku 5.5 | `claude-haiku-5-5` | `api.anthropic.com` |

- **Model IDs and endpoints are editable** (Settings → *Model* and *Advanced: API endpoint*) because providers rename models and run regional endpoints. International Qwen accounts and mainland Kimi accounts, for example, need a different endpoint.
- **Consent is per provider.** You are asked before anything is sent, and you can withdraw at any time.
- **What is sent:** the one word or sentence being checked, your answer, relevant known meanings/reference answers and the HSK level label. **Never sent:** your full vocabulary list, statistics, history or other settings.
- **Where keys live:** in `sessionStorage` by default (gone when you close the tab); "Remember key on this device" uses `localStorage`. Keys are **never** included in backups. Don't use "remember" on a shared computer.
- **Direct browser connection:** requests go straight from your browser to the provider — HanziFlow has no server. Some providers may block direct browser requests (CORS) or `file://` pages. If *Test connection* can't reach a provider, serve the file locally (Option B above) or set a proxy under *Advanced → API endpoint*.
- AI can be wrong. A failed or unclear AI check is **never** counted as correct automatically.

## Keyboard shortcuts

| Key | Action |
|---|---|
| `Enter` | Submit an answer |
| `Enter` or `N` | Next question after feedback |
| `Esc` | Exit practice (with confirmation) / close a dialog |
| `Tab` / `Shift+Tab` | Move between controls |
| Arrow keys | Move between tabs and selection groups |

Enter is ignored while a Chinese IME is composing, so confirming a character never submits your answer by accident.

## Data, backups and privacy

- Data is stored in your browser (IndexedDB, with a `localStorage` fallback) for the origin you open the app from. No account or server is involved.
- Clearing site data, private browsing, or switching browser/profile can make saved data unavailable.
- **Download a full backup (JSON)** from *Settings → Data* before clearing data or changing computers. Restoring also accepts HanziFlow 2.0 statistics exports. A safety snapshot is saved before anything is replaced.
- Upgrading from v2.0: your old data is copied forward and the originals are left untouched.
- CSV exports include a UTF-8 BOM so spreadsheet programs display Chinese correctly.

## Troubleshooting

| Problem | Try this |
|---|---|
| Progress isn't saved | Avoid private browsing; check that storage isn't blocked or full; use the same browser, profile and launch URL every time; export backups regularly. |
| Chinese shows as squares | Install a CJK font for your operating system. |
| A spreadsheet won't import | Check it's a valid, unprotected `.xlsx`/`.xls`/`.csv`; use the column mapper; read the import report for invalid rows and duplicates. |
| An answer seems wrong | HanziFlow checks against the meanings in your spreadsheet. Fix the source entry, or accept an alternative to save it separately. |
| AI says it can't connect | Check the key, model ID and endpoint; try Option B (local server); some providers need a proxy for browser requests. |
| "Update your browser" message | Use Chrome/Edge 123+, Firefox 120+ or Safari 17.5+. |

## Project structure

The app ships as **one self-contained file**:

| File | Purpose |
|---|---|
| `HanziFlow_v3_5_1.html` | The whole application — interface, logic, styles, bundled fonts and the SheetJS spreadsheet parser |
| `LICENSE` | Apache License 2.0 |

There is no package manager, build step or automated test suite. To test a change: open `index.html`, import a small spreadsheet, try every practice mode, reload to confirm persistence, then check all three designs in light and dark, in English and Chinese, at desktop and phone widths, and with reduced motion on.

## Contributing

Bug reports and focused improvements are welcome. Please include your browser and OS, whether you opened the file directly or through a local server, steps to reproduce, and (for import or answer-checking issues) a small anonymised sample spreadsheet. Please don't attach private vocabulary lists unless you're comfortable sharing them.

## Support HanziFlow ☕

HanziFlow is completely free, works fully offline, and has no ads, accounts or tracking. If it helps your Chinese study, you can help keep it improving with a small coffee:

[![Support on Ko-fi](https://img.shields.io/badge/Ko--fi-Buy%20me%20a%20coffee-ff5e5b?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/fardinfahim63404)

Starring the repository and sharing it with other learners helps too. 谢谢！

## Copyright and license

Copyright (c) 2026 Fardin Mahmud Fahim.

HanziFlow is licensed under the **Apache License, Version 2.0**. You may use, reproduce, modify and distribute it in accordance with the license. See [`LICENSE`](LICENSE) or <https://www.apache.org/licenses/LICENSE-2.0>.

When redistributing the project or a derivative work, include a copy of the license, retain applicable copyright, patent, trademark and attribution notices, and mark files you have changed. The Apache License does **not** require you to publish modified source code.

The HanziFlow license does not replace the licenses of bundled third-party components (for example SheetJS and the bundled fonts). Their notices are listed inside the app under **Third-party software and font licenses**. AI provider names and model names belong to their respective owners; HanziFlow is not affiliated with or endorsed by any of them.

---

<p align="center"><b>HanziFlow</b> — practise Chinese at your own pace, with your data kept on your device.</p>
