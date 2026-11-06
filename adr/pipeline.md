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

## Source encodings
- Decode MuseScore-exported lead sheets through a Leland decoder reading vector glyphs, stems and beams, one more entry in the 1-per-encoding table, with no new dependency. Why: the music glyphs are standard SMuFL characters, and a page image adds only error.
- Keep a mid-song key change in the score and move it by the song's own interval. Why: the change belongs to the song.

## Orchestral score
- Port an orchestral source as every staff printed, in its original key, with transposing instruments kept, to the 2 MuseScore files and 1 PDF only. Why: the file is the whole score, and the piano sheet and favourite tone do not apply.
- Exempt an orchestral score from the size rule and the line-layout rules, keeping the source's system breaks. Why: a full score reads system by system.

## MuseScore 3 file
- Build no `-ms3` file and remove any published one while the owner defers them; trigger to resume: the owner asks for MuseScore 3 files again, and every rule below applies unchanged then. Why: the file serves only sharing with older installs, and its build slows every song.
- Build each `-ms3` file with MuseScore 3.6.2 from the pipeline's own MusicXML, never from a MuseScore 4 export. Why: the export drops chord extensions.
- Hold notes, chords, chord beats and jumps identical to the latest file through the gate; layout may differ. Why: version 3 lays music out differently.
- Withhold a file that cannot match, and report the song and reason. Why: a wrong file is worse than a missing one at a rehearsal.

## Build cost
- Key every song's build and gate result on a digest of its source, the code and the style, and skip what is unchanged. Why: adding 1 song or fixing 1 sheet type must not rebuild the rest.
- Key every cache on what it was built from, never on a name or a path. Why: a name-keyed import from older code was reused and refused every layout.
- Store the embedded chord list by name in every published score's style, never by a build path. Why: a build path is machine-local and resolves nowhere else.
- Publish each song the moment it passes its gate, not at the end of a run. Why: a finished song owes the reader nothing further.
- Build each song in its own staging folder, copied into the sheets repo only under the publish lock. Why: publishing resets that repo, wiping any other song still building there.
- Gather the renders many threads ask for into 1 MuseScore job, and run what 1 sheet decides inside 1 MuseScore process through the extension. Why: MuseScore has no server mode, and each launch costs about 3 seconds.
- Fail fast in order of cost: preflight every song in seconds (decode, key, piano parts, tests, lint, control anchors), then the layout, then the gate as confirmation. Why: a defect visible in seconds must not wait hours.
- Stop a song at its first refusal and let other songs continue; refuse a chain whose tests or controls fail before any build. Why: 1 song must not block the rest, and a failed check must never run on.
- Size worker count to the free cores, and report build time per song. Why: oversubscribing the machine makes every process slower.
- Queue songs singly so a free worker takes the next song, publish 1 song at a time under a lock, and run planted-bug controls in parallel. Why: a fixed batch waits on its slowest song, and publishing rewrites shared history.
- Build songs in the owner's priority order, kept in private `muse-spike`, then the rest by name, and publish from the 1 process that collects finished songs. Why: the songs wanted first arrive first, and 1 collector serialises publishing without a lock file.
- Prove every pipeline change end to end on 1 song before a full run, and never change the code a running build loads. Why: each untested change landed mid-run and cost hours of builds.

## MuseScore Studio exports
- Read a staff as a second staff WHEN its bar lines fall where the staff above has them. Why: some lines print an extra part and others do not.
- Read a mid-song key or time change from signs beside no note. Why: these songs change key late.
- Read 2 segno signs as target then jump, and 2 coda signs as jump then target. Why: these sheets print bare signs without words.
- Cross-check every export with homr, as every other source. Why: a vector read and an image read share no failure.

## Song file
- Extract each source once into 1 JSON song file in private `muse-spike`, holding musical facts such as beats and no PDF positions, verified at extraction. Why: every later stage then needs only this file, so a decoder change rebuilds nothing.
- Apply a correction by editing the song file. Why: a fix to the music has 1 home and its own history.
- Key a build on the song file, the layout rules and the style, never on decoder or check code. Why: a change that cannot alter a sheet must not rebuild it.

## Calculated layout
- Measure each sheet once per candidate size, all sizes in 1 MuseScore job, with the page so wide and the last line so unjustified that every bar prints at its natural width, and run the extension on that render in its measure mode, which writes back through a score tag the share each bar must grow to hold its chords apart by the clear gap. Why: natural widths and chord needs predict line breaks, so lines are planned in code rather than found by trial renders.
- Plan each section's line breaks in code as 1 optimal search over every break: fewest lines first, then the most even fill to 93% of a line's width, with a cost on a lone bar and on a 2-bar line, never a break after a tied bar, never a line opening on a bar without a chord, and on a piano sheet a mark bar joined to the label block after it; budget each bar at its natural width grown by its chord share and each empty bar at the widest empty bar of its line, and choose the size from the plan. Why: every line rule holds by construction, where greedy packing needed a repair for each rule, and code decides in milliseconds.
- Change line starts only through the plan: when a render shows a line wrapped or still crowded after seating, grow the width estimate of that line's bars and plan again, so every line rule holds on every pass. Why: moving 1 bar by hand strands lone bars that no rule sees.
- Turn off automatic placement for every chord symbol and jump sign. Why: placed chords then never set a bar's width, and nothing moves what code placed.
- Fit and seat chords inside MuseScore with the pipeline's own extension, run through `--extension`, laying the score out again after each change in the same process: it widens through `userStretch` each bar whose chords lack the clear gap to the next chord, evens a line's empty bars to its widest, moves every beat-1 chord to its barline or line start, starts a line's first chord after any sign there, and lifts each line's chords on 1 baseline clear of cues, brackets and voltas. Why: 1 launch measures and places what render-and-parse took dozens of launches to approach.
- Pass the clear gap in staff spaces, and each chord's measured offset between its text box and its ink, to the extension through score tags, and run it again until the ink sits on target. Why: the gap has 1 home in the code, and the extension sees text boxes while the gate judges ink.
- Judge crowding, chord collisions and text on text only on a seated layout, never on a plan or a wrap correction. Why: unseated chords collide by construction, so an earlier check refuses every layout.
- Fail a measurement that cannot read what it measures, never return a neutral value in its place. Why: a zero chord need packed 12 bars on 1 line and read as a seating fault.
- Install the extension from the code repo into MuseScore's extensions folder before a build. Why: a build must not depend on a hand-installed copy.

## Measured MuseScore behaviour
- `mscore -j` loads every score of a job file in 1 process; an entry takes a list of outputs and a `transpose` object, and nothing keeps the program running between jobs.
- `mscore --extension <uri>` runs an installed extension on each loaded score before it is written; the extension reads every element's page position and box, moves offsets, and sees the layout redone after `endCmd`.
- A chord symbol with automatic placement widens its bar to fit its name, whatever `minHarmonyDistance` and `harmonyHarmonyDistance` say; with automatic placement off it never does, and nothing lifts it clear of cues, brackets or voltas.
- A page 200 inches wide with `lastSystemFillLimit` 1 prints a song on 1 line at natural bar widths; a page of ordinary width stretches every bar to fill its line.
- A bar's `stretch` only widens it: a bar a repeat barline or another fixed element holds wide never narrows.
- A score tag (`metaTag`) carries values both ways between the pipeline and its extension: `curScore.metaTag` reads 1 and `setMetaTag` writes 1 into the saved file.
- A bar's `userStretch` is writable from the extension and lays out again after `endCmd`.
- Automatic placement widens a bar for its chords with less than the clear gap: a natural render with it on still printed 29 chord collisions, so the pipeline sets chord room itself.
- Chords that overlap in a render merge into 1 text span in the PDF, so chord names are never counted from the spans of a render whose chords can overlap.
- The extension sees a chord's text box and the gate judges its ink; the 2 differ by up to about 0.5 point a chord, so each chord's offset is measured after 1 run and passed back.
- Some chords at a line start move several times the distance their offset asks, so a correction computed from 1 fixed scale overshoots.
- Lines are grouped by page and height; grouping by height alone merges the first lines of 2 pages.
- A line's first chord starts after the clef, the key and any sign printed there; a start-repeat barline does not move it.

## Class mechanisms
- Compare every word in a source's text layer with the decoded score in preflight, excluding only credit words, chord fragments and jump words. Why: a lost cue passes every check built on the decoder's own reading.
- Check every planted-bug anchor in preflight, in seconds, before any control or build runs. Why: a stale control must not stop a chain late.
- Tie a saved layout to a digest of the layout code and style, never a hand-raised number. Why: a forgotten bump republishes a stale sheet.
- Report published state only from `muse.status`, which reads the private repo's HEAD and the build records. Why: a state recalled from a run is not a state checked.

## Dependencies
- PyMuPDF: reads glyph positions and vector paths. Rejected: poppler text output, which has no path data. Maintenance: 2.0, inside the window.
- MuseScore 4.7.6: import, transposition, export. Maintenance: current release line.
- homr 0.7.0: independent cross-check only. Rejected as reader: returns no chords or text. Maintenance: active.
- Rejected: Audiveris 5.11, 53 of 62 bars exact on the comparison page and no chords. Rejected: oemer, last release outside the window. Rejected: PDFtoMusic Pro, paid and unneeded.

- MuseScore 3.6.2: the `-ms3` file only. Exception to latest-only and the 6-month window, because the file exists to serve version 3; used only when the MuseScore 3 deferral lifts; trigger to revisit: nobody needs version 3. Runs under Rosetta on Apple silicon.
- pytest, ruff, mypy: tests, lint, types in `muse`. Maintenance: active.

## Mechanism
- MuseScore 4 import keeps jump words and signs as staff text, so the port rewrites them into jump elements and markers carrying `<sym>`, `markerType`, `markerCenterOnSymbol` and `musicSymbolSize`. An untyped marker prints at the end of its bar.
- MuseScore 3 import writes the signs and jumps itself, so the port only sizes them.
- Embed the chord list in each `-ms3` file's style, because that program otherwise prints a slash chord from its shipped list as a staircase.
- The MuseScore 3 style adds `staffFontFace` and `systemFontFace`, MuseScore 3's own names for 2 text styles.
- A chord kind is written as `other` with its text, because a standard kind drops extensions on import.

- Judge chords against the measure boxes the score program exports with `.mpos`, reading only chord ink from the PDF. Why: a barline guessed from drawn strokes confuses stems, beams and short staves.

## Refusals
- Refuse an unknown music encoding by naming its fonts. Why: a wrong decoder reads confident garbage.
- Refuse an undecoded music symbol. Why: skipping it silently loses music.
- Refuse a missing SF Pro Display. Why: MuseScore otherwise falls back to another font without saying so.

## Gate
- Pass `muse.gate` before publishing: bars, pitches, chords, keys, navigation, inverse transposition, removed text, fonts, chord gap, margins, title, A4, ink edges, ties over line ends and a homr cross-check. Why: every rule lives in 1 command that states all failures at once.
- Check chord beats against the rule in `output-content.md`, and the built-in, boxed, wordless form of jump signs, on every MuseScore file built. Why: a chord in the right bar can still play late.
- Report a tie over a line end only where its tied run is too long for 1 line. Why: there the break is forced.
