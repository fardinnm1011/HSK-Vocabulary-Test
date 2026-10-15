# HanziFlow

**An offline-first Chinese vocabulary trainer for HSK study.** Import your own vocabulary spreadsheet, practise in multiple directions, review difficult words, and keep your learning history locally in your browser.

HanziFlow is built with plain HTML, CSS, and JavaScript. It requires no account, backend, build process, or runtime installation. The bundled SheetJS library enables spreadsheet import while offline.

> **Privacy by design:** your vocabulary, settings, and practice progress are stored in your browser's local storage. HanziFlow does not need to send your study data to a server.

## Highlights

- **Offline-first:** run locally after extracting the release files.
- **Five practice modes:** Chinese → English, English → Chinese, Pinyin practice, Pinyin → English, and Mixed practice.
- **Mistake review:** practise words you frequently miss, with harder words weighted more heavily.
- **Spreadsheet import:** import `.xlsx`, `.xls`, and `.csv` vocabulary files; select a worksheet, map columns, preview the import, and inspect invalid rows.
- **Flexible answer checking:** normalisation for English answers, Hanzi validation, Pinyin tone handling, and configurable strictness.
- **Vocabulary management:** browse, filter, select, and practise chosen words.
- **Progress tracking:** session and all-time summaries, practice history, per-word progress, and review data.
- **Local exports:** export vocabulary as CSV and statistics as JSON or CSV.
- **Personalisation:** light/dark/system theme support, adjustable practice settings, and optional sound feedback.
- **Responsive interface:** redesigned navigation and layouts, animated transitions, and reduced-motion awareness.
- **No build step:** open the page directly in a modern browser, or serve the folder locally if your browser restricts local-file features.

## Getting started

### 1. Download and extract

Download the latest HanziFlow release and extract the archive. Keep the files together in the same directory.

### 2. Launch the app

**Option A — open directly**

Double-click `index.html` to open it in your default browser. A recent version of Chromium/Chrome, Firefox, or Edge is recommended.

**Option B — use a local server**

If direct `file://` access causes import or browser-storage issues, open a terminal in the extracted folder and run:

```bash
python -m http.server 8000
```

On Windows, `py -m http.server 8000` may be used instead. Then visit [http://localhost:8000](http://localhost:8000).

The server is only a way to serve the files; HanziFlow itself remains a client-side application and can be used offline once the files are present.

**Important:** browser storage for `file://` and `http://localhost:8000` is separate. Choose one launch method and continue using it if you want to keep seeing the same saved progress.

### Linux note

On Arch Linux, you can open the file with `xdg-open index.html`, or use Firefox/Chromium. If Chinese characters render as boxes, install CJK fonts, for example:

```bash
sudo pacman -S noto-fonts-cjk
```

## Importing vocabulary

Choose **Replace or add file** on the Dashboard and select a supported spreadsheet. HanziFlow supports:

- `.xlsx`
- `.xls`
- `.csv` (UTF-8 recommended)

The bundled `xlsx.full.min.js` file is loaded when spreadsheet import is needed. Keep it beside `index.html`; it is included so importing can work without an internet connection.

### Spreadsheet columns

At minimum, include columns for Chinese characters, Pinyin, and English meaning. A `Level` column is optional and can be used for level-based filtering.

| Chinese (Hanzi) | Pinyin | English Meaning | Level |
|---|---|---|---|
| 学习 | xuéxí | to study; to learn | HSK 1 |
| 朋友 | péngyou | friend | HSK 1 |
| 重要 | zhòngyào | important | HSK 2 |

Header recognition is flexible and supports common alternatives such as `Chinese`, `Hanzi`, `汉字`, `拼音`, `Pinyin`, `Meaning`, `English`, and `英文`. If HanziFlow cannot confidently identify the columns, use the column mapper and specify the header row.

### Import behaviour

- Empty rows are ignored.
- Rows missing required fields are reported with spreadsheet row numbers and are not imported.
- Duplicate entries with the same Hanzi and Pinyin are counted and skipped; meanings may be merged into the existing entry.
- The same Hanzi with different Pinyin can be kept as separate entries, which is useful for polyphones such as 行 (*xíng* / *háng*).
- Meaning alternatives can be separated using supported punctuation such as semicolons, commas, or slashes; separators inside brackets are treated specially.
- **Replace** replaces the current vocabulary list; **Add** merges imported entries into the current list.

### Progress when updating a word list

A word's progress is associated with its Hanzi and Pinyin identity. Editing only the English meaning keeps its progress; changing Hanzi or Pinyin makes it a different word. Re-importing does not automatically erase historical progress. Back up important data before clearing browser storage or resetting the app.

## Practice modes

| Mode | Prompt | Your answer |
|---|---|---|
| Chinese → English | Hanzi, with optional Pinyin | English meaning |
| English → Chinese | English meaning | Hanzi |
| Pinyin practice | Hanzi | Pinyin |
| Pinyin → English | Pinyin | English meaning |
| Mixed practice | A random mix of selected directions | Depends on the question |
| Review mistakes | Words you have struggled with | Depends on the question |

Depending on the mode and settings, you can use hints, reveal an answer, skip a question, show or hide Pinyin, reshuffle a round, or exit practice. Review mode prioritises words that need more work.

### Answer checking

**English**
- Case, spacing, and punctuation are normalised.
- Leading articles such as “to”, “a”, or “the” and bracketed notes may be ignored.
- Normal mode accepts a listed meaning; strict mode can require all meanings.
- Lenient checking can forgive small typos on longer words.
- Partial words and unrelated answers are not accepted.

**Hanzi**
- Spaces are ignored, but characters must otherwise match.
- Simplified and traditional Chinese are not automatically converted.
- Some annotations and filler dots may be treated as optional.

**Pinyin**
- Case, spaces, and apostrophes are normalised.
- `v` and `u:` can represent `ü`.
- Tone marks are supported; tone-number input (for example, `xue2xi2`) depends on the relevant setting.
- Strict Pinyin checking requires the correct tones. When strict checking is disabled, tones are ignored.
- If tone numbers are disabled, entering them produces guidance without consuming an attempt.

Empty answers and formatting problems do not consume an attempt. English → Chinese checks Hanzi rather than accepting Pinyin as a substitute; an optional stricter setting can also require Pinyin.

### Attempts, hints, and scoring

Questions use a configurable wrong-answer limit. The default is three attempts, and the allowed range can be adjusted in Settings. Once the limit is reached, HanziFlow reveals the answer. Correct answers finish the question immediately, and each question is scored only once. Hints, giving up, and skipping are tracked separately. Session accuracy is based on correct answers relative to completed, revealed, and given-up questions.

### Keyboard shortcuts

- `Enter` — submit an answer.
- `Enter` or `N` after feedback — move to the next question.
- `Esc` — exit practice, with confirmation.
- `Tab` / `Shift+Tab` — move between controls.

Enter is ignored while a Chinese IME is composing text, so confirming an input character does not accidentally submit the answer.

## Dashboard and vocabulary management

The Dashboard summarises vocabulary, current-session activity, accuracy, and words needing review. It also provides import, export, and reset actions.

The Vocabulary view lets you browse and filter the imported list, select entries, and practise selected words. Large lists are paginated to keep the interface manageable.

## Data storage, privacy, and backups

HanziFlow stores vocabulary, settings, per-word progress, and practice history in the browser's `localStorage` for the origin from which it is opened.

- No account or server is required.
- Clearing browser site data, using a private browsing session, or changing the browser/profile can make saved data unavailable.
- Browser privacy settings or restrictions can prevent storage, especially for local files.
- Export statistics as **JSON** or **CSV**, and export vocabulary as **CSV**, to keep your own backups.
- CSV exports include a UTF-8 BOM to help spreadsheet programs display Chinese characters correctly.

**Recommended:** export your vocabulary and statistics before resetting progress, clearing site data, or changing computers/browsers.

## Settings and appearance

Use **Settings** to customise the supported practice behaviour, including answer checking and attempts. The theme control cycles through the available appearance preferences; the interface supports light, dark, and system themes. HanziFlow also uses animations for navigation, feedback, and theme changes where supported, while respecting reduced-motion preferences.

## Project files

The release archive contains these core files:

| File | Purpose |
|---|---|
| `index.html` | Application document and dialog structure |
| `style.css` | Layout, responsive styling, themes, and animations |
| `app.js` | Importer, answer checking, quiz engine, storage, and UI logic |
| `xlsx.full.min.js` | Bundled SheetJS spreadsheet parser used for offline imports |

The previous development archive may include a sample workbook; the v2.0 release archive does not include that workbook, so prepare your own spreadsheet or retain a copy of the sample from the earlier release if you need test data.

## Troubleshooting

**The Excel reader did not load**
- Make sure `xlsx.full.min.js` is in the same folder as `index.html`.
- If you opened the app from a local file, try the local-server method above.
- The library is loaded on demand for import; it does not need to be fetched from the internet.

**Progress is not saved**
- Avoid private browsing.
- Check whether browser storage is blocked or full.
- Use the same browser, profile, and launch URL each time.
- Export your data regularly.

**Chinese characters appear as squares**
- Install an appropriate CJK font for your operating system.

**A spreadsheet will not import**
- Confirm that it is a valid `.xlsx`, `.xls`, or `.csv` file and is not password-protected.
- Check the required columns and use the column mapper if automatic detection fails.
- Review the import report for invalid rows and duplicates.

**An answer seems incorrect**
- HanziFlow checks against the meanings in your spreadsheet. Review the source workbook if an entry has an unexpected meaning or spelling.

## Development and testing

HanziFlow is a static client-side project; no package manager or compilation step is required.

For a quick local test:

1. Launch `index.html` directly or run a local HTTP server.
2. Import a small vocabulary spreadsheet.
3. Test each practice mode and its answer-checking settings.
4. Verify that incorrect answers, hints, skip, give-up, and answer reveal behave as expected.
5. Navigate away and return, then reload the app to check persistence.
6. Test light/dark/system appearance, keyboard navigation, and reduced-motion preferences.
7. Test offline use after extracting all release files.

There is no automated test command or package manifest included in this release archive.

## Contributing

Bug reports and focused improvements are welcome. When reporting an issue, include:

- Browser and operating system.
- Whether the app was opened as a local file or through a local server.
- Steps to reproduce the issue.
- A small, anonymised example spreadsheet if the issue concerns importing or answer checking.

Please do not attach private vocabulary lists unless you are comfortable sharing their contents.
an explicit open-source license. The bundled SheetJS library has its own license; see its upstream project and the distributed library notices for details.

---

**HanziFlow** — practise Chinese vocabulary at your own pace, with your data kept on your device.
