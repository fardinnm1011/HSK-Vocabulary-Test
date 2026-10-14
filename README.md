# HanziFlow

An offline Chinese vocabulary trainer. Plain HTML, CSS and JavaScript — no install, no build step, no server, no account, no internet. Import your own Excel vocabulary file and practise in five modes with a three-attempt answer system. Nothing you import or answer ever leaves your computer.

## Files

| File | Purpose |
|---|---|
| `HanziFlow.html` | Page structure and dialogs |
| `style.css` | Styling, light/dark themes, responsive layout |
| `app.js` | All logic (import, answer checking, quiz engine, storage, UI) |
| `xlsx.full.min.js` | SheetJS 0.18.5 (Apache-2.0), bundled so Excel import works offline |
| `sample_vocabulary.xlsx` | A small test workbook (clean sheet + an "Edge Cases" sheet) |

## Launching

**Windows 11:** unzip the folder, double-click `index.html` (it opens in your default browser). Edge, Chrome and Firefox are all fine.

**Arch Linux:** unzip, then `xdg-open index.html`, or `firefox index.html` / `chromium index.html`.

That is all. The app does not need Python or Node.

### If something misbehaves when opened as a file
Browsers treat `file://` pages specially. This app was tested in Chromium opened directly from disk (import, quizzes and saved progress all worked), but I can't promise every browser version behaves the same. If importing or saving fails, serve the folder locally instead (the server is only a delivery method; the app still runs offline):

```
cd HanziFlow
python -m http.server 8000      # Windows: py -m http.server 8000
# then open http://localhost:8000
```

Saved progress belongs to the address you open the app from. `file://…/index.html` and `http://localhost:8000` are different "sites" with separate saved data, so pick one way and keep using it.

## Excel format

Three columns are needed; extra columns (such as `No.`) are ignored. A fourth, optional `Level` column enables level filters.

| Chinese (Hanzi) | Pinyin | English Meaning |
|---|---|---|
| 学习 | xuéxí | to study; to learn |

* Headers are matched case-insensitively and ignore punctuation: *Hanzi, Chinese, 汉字, Pinyin, 拼音, Meaning, English, 英文* and similar all work.
* If headers aren't recognised, the import window shows a column mapper (and a "header row number" box). It also appears pre-filled whenever you want to override the guess.
* Several meanings can be separated by `;` `,` or `/` (ASCII or full-width). Separators inside brackets are ignored.
* Multiple worksheets: choose one in the import window. `.xlsx`, `.xls` and `.csv` (UTF-8) are accepted.
* Empty rows are ignored. Rows missing Hanzi, Pinyin or meaning are **listed, not silently dropped**, with row numbers, and are not imported — fix them in Excel and re-import.
* Duplicates (same Hanzi **and** same Pinyin) are skipped and counted; any extra meanings in the duplicate are merged into the first copy. Same Hanzi with a different Pinyin (e.g. 行 xíng / háng) are separate words.

### Progress and re-importing
Progress is tied to a word's identity = Hanzi + Pinyin (tone marks included, spaces ignored). Importing a new or edited file never deletes progress. Editing only the English meaning keeps a word's progress; changing its Pinyin or Hanzi makes it a new word (the old record stays saved but unused). "Replace" swaps the word list; "Add" merges into it.

## Modes
* **Chinese → English** – see Hanzi (Pinyin optional), type the meaning.
* **English → Chinese** – see English, type Hanzi. Pinyin alone is never accepted. Optional strict mode also requires correct Pinyin.
* **Pinyin practice** – see Hanzi, type Pinyin.
* **Pinyin → English** – see Pinyin, type the meaning. Because many words share a Pinyin, meanings of same-Pinyin words are also accepted by default (switchable in Settings).
* **Mixed** – random mix of the directions you tick.
* **Review mistakes** – practise words you miss often, weighted toward the worst.

## How answers are checked
* **English:** ignores case, spacing, punctuation, leading "to/a/the" and bracketed notes. *Normal*: any one listed meaning, or a list of valid meanings. *Strict*: all meanings. *Lenient*: also forgives small typos on longer words. Partial words ("stud") and unrelated text are never accepted.
* **Hanzi:** spaces ignored; every character must match. Simplified/traditional alternatives are **not** converted (that needs a character table, which this app does not bundle), so 學习 ≠ 学习. Annotations like （名词） and filler dots/ellipses are optional.
* **Pinyin:** case, spaces and apostrophes are ignored; `v`/`u:` mean ü. Strict (default) requires the right tones; tone numbers (`xue2xi2`, 5/none = neutral) can be allowed or switched off. If tone numbers are off and you type one, you get a message and **no attempt is used**. With strict Pinyin off, tones are ignored.

Empty answers and formatting problems never use an attempt.

## Three-attempt rule
Each question allows 3 wrong answers (configurable 1–10; the dots and text follow your setting). After the last one the answer is revealed. A correct answer ends the question at once; a question is scored only once. Hint and Give up are counted separately from wrong answers; Skip is unscored. Accuracy = correct ÷ (correct + revealed + gave up).

## Keyboard
`Enter` submit · after feedback `Enter` or `N` = next (a short guard prevents the submitting Enter from also advancing) · `Esc` exit practice (asks first) · `Tab`/`Shift+Tab` for everything else. Enter while a Chinese IME is composing is ignored, so confirming a character never submits.

## Your data
Stored in your browser's localStorage: vocabulary, settings, per-word progress, session history. **Clearing site data or using a private window deletes it.** Use *Export stats* (JSON/CSV) and *Export vocabulary* (CSV) on the dashboard for backups. Export CSVs include a BOM so Excel shows Chinese correctly.

## Troubleshooting
* **Chinese shows as boxes (Arch):** `sudo pacman -S noto-fonts-cjk`.
* **"Excel reader did not load":** `xlsx.full.min.js` must sit next to `index.html`.
* **Progress not saved:** the app warns if storage is blocked. Don't use a private window; try the local-server method.
* **File won't import:** it must be a real `.xlsx/.xls/.csv`; password-protected or renamed files are rejected with a message.
* **Weird answers in your own list** (e.g. 行 meaning "That's ok") come from the spreadsheet; the app can only check against what the file says. Fix them in Excel and re-import.

## Architecture
`app.js` is one file in layers: **Text** (normalisation + validation), **Importer** (column detection + parsing + reports), **Quiz** (index of homophones/polyphones, shuffled queue, attempt engine, tallies), **Store** (corruption-safe localStorage), then the **UI** (rendering and delegated events). The first four contain no DOM code and were unit-tested in Node.

## Testing checklist
- [ ] Import `sample_vocabulary.xlsx`: sheet chooser shows 3 sheets; "Edge Cases" shows 6 valid, 2 duplicates, 2 invalid, 1 empty row.
- [ ] Import a file with unrecognisable headers: mapper appears with a warning; Import is disabled until all columns are set.
- [ ] Import a `.txt`, a renamed non-spreadsheet and an empty sheet: clear errors, nothing imported.
- [ ] Re-import (Replace and Add): progress is kept; no duplicates in Add.
- [ ] Chinese → English and English → Chinese: correct, wrong, and 3-wrong reveal; Pinyin-only answer rejected in English → Chinese.
- [ ] Pinyin practice: `xuéxí` ✓, `xue2xi2` ✓ (when allowed), `xuexi` ✗ in strict, ✓ when strict is off; wrong tone ✗.
- [ ] Pinyin → English: correct meaning ✓; homophone meaning ✓ only with the setting on.
- [ ] Messages: "Not quite… 2 attempts remaining", "Still incorrect… 1 attempt remaining", then reveal; answer is never shown earlier.
- [ ] Enter submits; Enter right after reveal does not skip; Submit is disabled once finished.
- [ ] Randomisation: no repeats until all words used; two rounds differ; Continuous reshuffles; Reshuffle works mid-round.
- [ ] Scoring: results screen numbers match what you did; hints/give-up/skip are separate.
- [ ] Persistence: close and reopen the browser — vocabulary, settings, stats, history, Review list remain.
- [ ] Offline: disconnect from the network (or open in a fresh profile offline) and run import + quiz.
- [ ] Reset session / reset all / clear vocabulary ask for confirmation; exports download.
