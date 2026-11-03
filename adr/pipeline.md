# Pipeline

Owns how a source PDF becomes a final sheet. The reader row is provisional until the comparison in `muse-spike` measures it.

| Stage | Pick | State | Disproved by |
|---|---|---|---|
| Read notes | Direct reader of PDF symbols, 1 decoder per encoding | locked | a bar disagreeing with the source when read by eye |
| Cross-check | homr 0.7 on every source page | locked | a real disagreement left unresolved |
| Read chords and text | Direct PDF text extraction | locked | a chord or cue missing from the output |
| Write | MusicXML with explicit system breaks | locked | a wrong import in MuseScore |
| Transpose | MuseScore `--transpose` | locked | a verifier mismatch on notes, chords or key |
| Export | MuseScore `mscore` with a fixed style file | locked | a header, footer or banned text in the PDF |

## MuseScore 3 file
- Build each `-ms3` file with MuseScore 3.6.2 from the pipeline's own MusicXML, never from a MuseScore 4 export. Why: the export drops chord extensions.
- Hold notes, chords, chord beats and jumps identical to the latest file through the gate; layout may differ. Why: version 3 lays music out differently.
- Withhold a file that cannot match, and report the song and reason. Why: a wrong file is worse than a missing one at a rehearsal.

## Dependencies
- PyMuPDF: reads glyph positions and vector paths. Rejected: poppler text output, which has no path data. Maintenance: 2.0, inside the window.
- MuseScore 4.7.6: import, transposition, export. Maintenance: current release line.
- homr 0.7.0: independent cross-check only. Rejected as reader: returns no chords or text. Maintenance: active.
- Rejected: Audiveris 5.11, 53 of 62 bars exact on the comparison page and no chords. Rejected: oemer, last release outside the window. Rejected: PDFtoMusic Pro, paid and unneeded.

- MuseScore 3.6.2: the `-ms3` file only. Exception to latest-only and the 6-month window, because the file exists to serve version 3; trigger to revisit: nobody needs version 3. Runs under Rosetta on Apple silicon.
- MuseScore 3.6.2: builds the legacy `-ms3` score. Exception to the active-maintenance window: its age is the purpose, and the last release of the line stays frozen until nobody needs the format. Trigger to revisit: the format stops being requested. Maintenance: none upstream; last release.
- pytest, ruff, mypy: tests, lint, types in `muse`. Maintenance: active.

## Mechanism
- MuseScore import keeps jump words and signs as staff text, so the port rewrites them into jump elements and hidden markers in the saved score.
- A chord kind is written as `other` with its text, because a standard kind drops extensions on import.

## Refusals
- Refuse an unknown music encoding by naming its fonts. Why: a wrong decoder reads confident garbage.
- Refuse an undecoded music symbol. Why: skipping it silently loses music.
- Refuse a missing SF Pro Display. Why: MuseScore otherwise falls back to another font without saying so.

## Gate
- Pass `muse.gate` before publishing: bars, pitches, chords, keys, navigation, inverse transposition, removed text, fonts, chord gap, margins, title, page cap, A4, ink edges, ties over line ends and a homr cross-check. Why: every rule lives in 1 command that states all failures at once.
- Check chord beats against the rule in `output-content.md`, and the built-in form of jump signs, on both MuseScore files. Why: a chord in the right bar can still play late.
- Report a tie over a line end only where its tied run is too long for 1 line. Why: there the break is forced.
