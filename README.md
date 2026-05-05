# Quran Hifz Practice

A lightweight, offline-capable browser app for practising Quran memorisation (hifz) by typing. Select a surah and ayah, type from memory, and get instant word-by-word feedback.

![screenshot placeholder](screenshot.png)

## Features

- **Full Quran corpus** — all 6,236 ayaat from the [Tanzil Simple Clean](https://tanzil.net/download/) text, embedded directly in the HTML; no server or internet required after first load
- **Surah and ayah selector** — pick any surah from a dropdown, navigate ayaat with previous/next buttons or type the number directly
- **Remembers your place** — the last surah and ayah you were on is restored automatically on your next visit
- **Word-by-word comparison** — after typing, each word is highlighted green (correct) or red (wrong/missing)
- **Auto-advance** — reaching 95% or above automatically moves to the next ayah after a brief pause
- **Hint** — reveals the first ~30% of the ayah when you're stuck
- **History panel** — scrollable log of all your attempts this session, showing what you typed vs the correct text with scores
- **Shared equivalent pairs** — site-wide `equivs.json` file defines variant spellings that apply to all users (e.g. ملك = مالك); update and redeploy to roll out to everyone
- **Personal equivalent pairs** — users can add their own pairs on top; stored in localStorage and persist across sessions
- **Urdu keyboard support** — normalises Urdu-script characters (ی، ک، ہ، ۃ) to their Arabic equivalents before comparison, so Pakistani keyboard users aren't penalised
- **Hamza normalisation** — أ، إ، آ are treated as equivalent to plain ا during comparison, so users aren't penalised for omitting hamza
- **No diacritics required** — the Tanzil Simple Clean corpus has no tashkeel; you type plain letters
- **On-screen Arabic keyboard** — built-in keyboard toggled with the ⌨ button, for users without an Arabic hardware keyboard; inserts at cursor position

## Usage

1. Open `index.html` in any modern browser
2. Select a surah from the dropdown
3. Navigate to the starting ayah using the number input or ‹ › buttons
4. Type the ayah from memory in the input panel
5. Press **Compare** (or Ctrl+Enter) to see your score
6. Press **Clear** to try again or move to the next ayah

### Equivalent Pairs

Some words have variant spellings depending on whether you follow رسم عثماني (Uthmani script) or standard Arabic orthography. You can teach the app to accept both:

1. Click **Equivalent pairs** at the bottom to expand the panel
2. Type one form in the first box and the alternate in the second
3. Press **Add** — the pair is saved immediately and applies to all future comparisons
4. Pairs are stored in your browser's localStorage and persist across sessions

Example pairs to add:
| Corpus (Tanzil) | Common variant |
|---|---|
| الرحمن | الرحمان |
| مالك | ملك |

## Deployment

The app is a single static HTML file with no dependencies or build step.

**GitHub Pages**
```
1. Rename the file to index.html
2. Push to a GitHub repository
3. Go to Settings → Pages → Deploy from branch (main / root)
4. Your app is live at https://yourusername.github.io/reponame
```

**Netlify Drop**
```
Drag the file onto netlify.com/drop — instant public URL, no account needed
```

**Any web server**
```bash
cp index.html /var/www/html/
# or with Python for quick local testing:
python3 -m http.server 8000
```

> **Note on localStorage:** equivalent pairs are scoped to the domain. If you access the app from multiple URLs (e.g. locally and via GitHub Pages), you'll need to re-add pairs on each. An export/import feature can be added if needed.

## Technical Details

| Item | Detail |
|---|---|
| Corpus source | [quran-json](https://www.npmjs.com/package/quran-json) npm package |
| Corpus preparation | Diacritics and Quranic annotations stripped at build time (Unicode ranges U+064B–U+065F, U+06D6–U+06ED, plus superscript alef U+0670 converted to explicit alef) |
| Input normalisation | Urdu-script characters mapped to Arabic equivalents; any diacritics in user input stripped before comparison |
| Matching | Word-by-word exact match on normalised strings, with optional user-defined equivalents applied |
| Font | [Amiri](https://fonts.google.com/specimen/Amiri) loaded from Google Fonts (falls back to Traditional Arabic / Arial Unicode MS if offline) |
| Storage | Browser localStorage for equivalent pairs only; no data is sent anywhere |
| File size | ~950 KB (Quran data ~870 KB embedded as JSON) |

## Building from Source

If you want to regenerate the HTML from the build script:

```bash
npm install quran-json
node build4.js
```

Output: `quran_hifz_v4.html`

## Roadmap

- [ ] Export / import personal equivalent pairs as JSON
- [ ] Session score tracking across multiple ayaat
- [ ] Handwriting input (tablet) via stylus + OCR — the original motivation for this project
- [ ] Surah range practice mode (e.g. practice ayaat 5–15 in a loop)
- [ ] و-prefix normalisation (user types و الشمس, corpus has والشمس)
- [ ] Mobile layout optimisation
