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
- Save each sheet's chosen size, line gap, chord width and line breaks, and start from them on a rebuild, re-proving them in 1 pass. Why: searching again repeats work already decided.
- Search a sheet's size and lines with chords left as laid out and bars only estimated, then seat chords and even bars on the chosen layout alone; if its final checks fail, step down 1 size, a few times at most. Why: a layout measured at every trial costs thousands of launches for a decision 1 render confirms.
- Refuse a sheet at the smallest size with a line gap above the base gap. Why: that height could have carried bigger notation, so the search failed.
- Record whether a sheet's layout was searched under the current layout rules or only re-proved; a re-proved sheet owes a later search for bigger notation. Why: a re-proof passes the gate but can only hold its size.
- Store the embedded chord list by name in every published score's style, never by a build path. Why: a build path is machine-local and resolves nowhere else.
- Cap each sheet at 150 MuseScore launches during its search, a piano sheet at 250 because it packs lines from the render and seeks the largest chords on 1 page, then keep the best layout found or refuse the sheet and list it. Why: 1 hard sheet must not hold the rest for hours.
- Publish each song the moment it passes its gate, not at the end of a run. Why: a finished song owes the reader nothing further.
- Build each song in its own staging folder, copied into the sheets repo only under the publish lock. Why: publishing resets that repo, wiping any other song still building there.
- Reuse 1 MuseScore process per batch wherever its job mode allows. Why: each launch costs seconds, and the search makes many.
- Fail fast in order of cost: preflight every song in seconds (decode, key, piano parts, tests, lint), then 1 layout per sheet through the final-PDF checks, then the search, then the gate as confirmation. Why: a defect visible in seconds must not wait hours.
- Stop a song at its first refusal and let other songs continue; refuse a chain whose tests or controls fail before any build. Why: 1 song must not block the rest, and a failed check must never run on.
- Size worker count to the free cores, and report build time per song. Why: oversubscribing the machine makes every process slower.
- Queue songs singly so a free worker takes the next song, publish 1 song at a time under a lock, and run planted-bug controls in parallel. Why: a fixed batch waits on its slowest song, and publishing rewrites shared history.

## MuseScore Studio exports
- Read a staff as a second staff WHEN its bar lines fall where the staff above has them. Why: some lines print an extra part and others do not.
- Read a mid-song key or time change from signs beside no note. Why: these songs change key late.
- Read 2 segno signs as target then jump, and 2 coda signs as jump then target. Why: these sheets print bare signs without words.
- Cross-check every export with homr, as every other source. Why: a vector read and an image read share no failure.

## Song file
- Extract each source once into 1 JSON song file in private `muse-spike`, holding musical facts such as beats and no PDF positions, verified at extraction. Why: every later stage then needs only this file, so a decoder change rebuilds nothing.
- Apply a correction by editing the song file. Why: a fix to the music has 1 home and its own history.
- Key a build on the song file, the layout rules and the style, never on decoder or check code. Why: a change that cannot alter a sheet must not rebuild it.
- Carry a saved layout across a code change by re-proving it in 1 pass, and search only when that proof fails. Why: a finished search stays paid for.
- Fail a re-proof when the saved lines break a line rule. Why: lines saved under older rules otherwise waste a build and fall at the gate.
- Start a search at the sheet's saved size and look near it, with a cap counted per search step rather than reached silently. Why: a cap reached mid-search reads as nothing fitting and drops the sheet to the smallest size.
- Climb to larger sizes when the smallest fail, before refusing a sheet. Why: text keeps its size, so the smallest notation can collide where a larger one fits.
- Render a search's candidate sizes through 1 MuseScore job, each score carrying its own style. Why: startup is about 3.3 of each launch's 3.4 seconds.

## Calculated layout
- Measure each sheet once per candidate size, all sizes in 1 MuseScore job, with the page so wide and the last line so unjustified that every bar prints at its natural width. Why: natural widths predict line breaks, so lines are planned in code rather than found by trial renders.
- Plan line breaks in code to 93% of a line's width, under every line rule, and choose the size from the plan. Why: a planned line held exactly on 7 of 8 sheets measured, and code decides in milliseconds.
- Render the plan, then correct from measurement until nothing is off: a wrapped line hands its last bar down, empty bars widen to the widest of their line, each chord moves by its own measured response. Why: bars only grow and some chords move twice the distance asked, so a fixed correction overshoots.
- Turn off automatic placement for every chord symbol and jump sign. Why: placed chords then never set a bar's width, and nothing moves what code placed.
- Seat chords inside MuseScore with the pipeline's own extension, run through `--extension`: it moves every beat-1 chord to its barline or line start, lifts each line's chords on 1 baseline clear of cues, brackets and voltas, and starts a line's first chord after any sign there, laying the score out again after each move in the same process. Why: 1 launch measures and places what render-and-parse took dozens of launches to approach.
- Pass each chord's measured offset between its text box and its ink to the extension through a score tag, and run it again until the ink sits on target. Why: the extension sees text boxes and the gate judges ink.
- Install the extension from the code repo into MuseScore's extensions folder before a build. Why: a build must not depend on a hand-installed copy.

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

- MuseScore 3.6.2: the `-ms3` file only. Exception to latest-only and the 6-month window, because the file exists to serve version 3; trigger to revisit: nobody needs version 3. Runs under Rosetta on Apple silicon.
- pytest, ruff, mypy: tests, lint, types in `muse`. Maintenance: active.

## Mechanism
- MuseScore 4 import keeps jump words and signs as staff text, so the port rewrites them into jump elements and markers carrying `<sym>`, `markerType`, `markerCenterOnSymbol` and `musicSymbolSize`. An untyped marker prints at the end of its bar.
- MuseScore 3 import writes the signs and jumps itself, so the port only sizes them.
- Embed the chord list in each `-ms3` file's style, because that program otherwise prints a slash chord from its shipped list as a staircase.
- The MuseScore 3 style adds `staffFontFace` and `systemFontFace`, MuseScore 3's own names for 2 text styles.
- A chord kind is written as `other` with its text, because a standard kind drops extensions on import.

- Seat and judge chords against the measure boxes the score program exports with `.mpos`, reading only chord ink from the PDF. Why: a barline guessed from drawn strokes confuses stems, beams and short staves.

## Refusals
- Refuse an unknown music encoding by naming its fonts. Why: a wrong decoder reads confident garbage.
- Refuse an undecoded music symbol. Why: skipping it silently loses music.
- Refuse a missing SF Pro Display. Why: MuseScore otherwise falls back to another font without saying so.

## Gate
- Pass `muse.gate` before publishing: bars, pitches, chords, keys, navigation, inverse transposition, removed text, fonts, chord gap, margins, title, page cap, A4, ink edges, ties over line ends and a homr cross-check. Why: every rule lives in 1 command that states all failures at once.
- Check chord beats against the rule in `output-content.md`, and the built-in form of jump signs, on both MuseScore files. Why: a chord in the right bar can still play late.
- Report a tie over a line end only where its tied run is too long for 1 line. Why: there the break is forced.
