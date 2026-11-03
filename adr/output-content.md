# Output content

Owns what a final sheet carries.

## MUST
#### What prints
- Keep notes, chords, tempo, section marks, repeats, band cues and hint lyrics. Why: the sheet is for performing.
- Print the song title small, once, at the top right of every page. Why: a printed sheet is found in a stack by its corner.
- Set all text in SF Pro Display, installed from Apple on every machine that edits a score. Why: it carries every Vietnamese letter in all 4 styles.
- Make chords the biggest text with at least 0.75 chord font size of clear space between 2 chords, giving bars with more chords more width. Why: chords are what gets read first.
#### Chords and signs
- Place each chord on its true beat: a lone chord in a bar without notes on beat 1, several chords in a bar without notes on real beats by their order, and a chord in a bar with notes on the note the source aligns it to. Why: a chord that looks right and plays late sounds wrong.
- Write segno and coda as the score application's own built-in signs, enlarged through its own size setting. Why: a raw character behaves as plain text.
- Print segno and coda signs at least 1.6 times the chord size, and the jump words To Coda and D.S. al Coda in bold below the chord size. Why: a performer must spot a jump at a glance; the signs are the only exception to chords being biggest.
#### Layout
- Keep the original-tone and favourite-tone PDFs to 2 pages at most. Why: more pages mean page turns while performing.
- Lay out lines for reading, independent of the source: start each line at 8 bars and halve only a line whose chords or notes do not fit, avoiding a stranded single bar, then a break on a tied note. Why: short bars stay compact and wide bars get room.
- Scale each song to the largest notation that fits 2 pages with at most 15% of lines holding 2 bars or fewer, with lines spread evenly. Why: bigger reads better and blank space serves nothing.

#### Source mistakes
- Correct a mistake in a source sheet wherever evidence inside the song settles the right notation: a repeated section, the harmony, the bar length, the sibling verse. Why: the output must be musically correct, not a copy of an error.
- List every correction per song with bar, source reading, corrected reading and evidence. Why: each correction stays checkable against the source.
- Flag a suspected mistake in that list, unchanged, when the song holds no evidence that settles it. Why: a guess presented as a correction is a new error.

- Put readability ahead of tightness: readable notation size, clear gaps between lines, small margins, lines spread evenly across pages, no ink at the page edge. Why: the sheet is read while performing.

## Chord timing
- Put a lone chord in a bar without notes on beat 1. Why: a chord in a silent bar plays with the bar's start.
- Space several chords in a bar without notes evenly over the bar by their order. Why: a printed position in an empty bar is not a timing.
- Align a chord in a bar with notes to the note it is printed over. Why: the chord sounds with that note.

## Signs
- Write segno and coda as MuseScore's built-in markers, `<sym>segno</sym>` and `<sym>coda</sym>`, enlarged by their size property. Why: MuseScore recognises only that form as a sign.

## Song folder
- Publish each song as a folder `<name>/` holding `<name>-<original>.mscz`, `<name>-<original>-ms3.mscz`, `<name>-<original>.pdf`, `<name>-<tone>.pdf` and `<name>-<tone>-piano.pdf`. Why: 1 place per song, in the tone it was written and the tone it is played.
- Keep MuseScore files in the original tone only. Why: the tone PDFs are for reading and the scores for editing.
- Skip `<name>-<tone>.pdf` when the original is already the tone. Why: it would be the same file twice.
- Hold the same rules on the original-tone PDF as on the tone PDF, measured with its own chord names. Why: chord names change width with the key.
- Build the latest score with the newest MuseScore installed and rebuild on request after an upgrade. Why: newer versions open older files, and a new version can change layout.
- Withhold an `-ms3` score whose notes, chords, chord beats or jumps differ from the latest score, and report it. Why: a wrong file is worse than a missing one.

## Piano PDF
- Print 1 staff per line, always. Why: a pianist voices chords freely.
- Print notes only in the intro, interludes and outro, and empty bars with chords and hidden rests everywhere else. Why: other instruments' notes are not the pianist's to read.
- Find unlabelled parts from the song's shape: bars before the first verse are the intro, instrumental bars between sung sections are interludes, bars after the last sung section are the outro. Why: most sheets label only some parts.
- Keep the staff cued as piano or keys in a 2-staff kept bar, otherwise the top melody, and list each choice in the private folder. Why: that is the line played.
- Keep every cue, section mark, jump, tempo and stop mark. Why: the pianist reads the whole band picture.
- Shorten hint lyrics to the first words. Why: they locate the singer and cost height.
- Fit 1 page, using 2 only when chords would fall below a readable size. Why: no page turn.

## NEVER
- Never print a composer or performer credit, copyright line, contact, watermark, header, footer or page number. Cost: the sheet stops being music only.

## Piano PDF
- Print 1 staff per line, in the favourite tone. Why: a pianist voices from chords.
- Keep notes only in the intro, interlude and outro; every other bar is empty with chords above and its rest hidden. Why: other instruments' notes are not the pianist's concern.
- Treat bars before the first verse or section mark as the intro, a labelled interlude or an instrumental stretch without lyric labels as an interlude, and bars after the last chorus section as the outro; list each inferred boundary per song. Why: many sheets do not label these parts.
- In a kept part with 2 staves, keep the staff cued Piano or Key, otherwise the top staff; list each choice per song. Why: the staff the pianist plays can sit on either line.
- Space 2 chords in an empty bar by beat position. Why: position shows the change without extra marks.
- Keep every cue, repeat, volta, jump sign, section mark, tempo and style; shorten each hint lyric to its first few words. Why: these are the map of the song and hint lines cost height.
- Fit 1 page, going to 2 only when chords would otherwise fall below a readable size. Why: 1 page needs no page turn.
