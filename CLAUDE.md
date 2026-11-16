# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Vocabulary trainer for the English textbook *Go Ahead, Realschule Bayern 2017, 5. Jahrgangsstufe* (Cornelsen), hosted as a GitHub Page (https://rollendetonne.github.io/Vokabeltrainer/). The UI, code comments and commit messages are in German.

## Running

No build, no dependencies, no tests, no linter. It is plain static HTML/JS. Because `index.html` loads data via `fetch("vokabeln.json")`, opening the file directly (`file://`) fails; serve the directory instead:

```
python3 -m http.server 8000   # then open http://localhost:8000
```

## Structure

- `index.html` – the whole app: CSS in `<style>`, all logic in one inline `<script>` (no frameworks, no modules).
- `vokabeln.json` – flat array of entries `{ id, unit, section, page, english, german, example }`.

### Vocabulary data

- The unit/section picker is built automatically from `unit` and `section` in order of first appearance, so new entries are appended to the end, grouped by unit/section, with the next sequential `id`.
- On load, the **last unit in the file** is preselected, so the file order determines the default selection.
- Learning progress is stored in `localStorage` under `goahead5-progress` as `id -> correct answers in a row` (a word counts as learned at ≥ 2). **Never renumber or reuse existing `id`s**: that would scramble users' saved progress.
- The `english`/`german` strings are not just display text. The checker in "Schreiben" mode parses them, so their notation matters (see below). Keep following the book's conventions.

### Answer checking (`checkAnswer` / `variants` / `norm` in `index.html`)

The check is deliberately strict about spelling and capitalization. It returns `ok`, `case` (only capitalization wrong), `spelling` (edit distance ≤ 1, or ≤ 2 for words longer than 6 characters) or `wrong`; only `ok` counts as correct, but the user can override. The accepted answers are derived from the target string:

- `,` `;` `/` at the top level (outside parentheses) separate alternative answers.
- `Freund/in`, `mein/e`, `Jugendliche/r`: `/in`, `/e`, `/r` suffixes accept both forms instead of being treated as alternatives.
- `(= …)`: the content is accepted as an extra variant (`I'm (= I am)`).
- `(Schul-)Heft` accepts `Heft` and `Schulheft`; `Mathe(matik)` accepts both forms; `(pl)` and everything from `pl` onward is dropped.
- In the German → English direction, a leading `to ` in the answer is optional (entries look like `(to) go`).
- `norm` ignores surrounding or repeated whitespace, trailing `.!?`, ellipses, quotation marks, and differing apostrophe characters.

When you change the vocabulary notation or the checker, test the affected entries against `variants()` (for example in the browser console).
