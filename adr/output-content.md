# Output content

Owns what a final sheet carries.

## MUST
#### What prints
- Keep notes, chords, tempo, section marks, repeats, band cues and hint lyrics, and drop a source text that is only punctuation. Why: the sheet is for performing, and a lone dot is print dust.
- Refuse a size where a segno or coda prints on the baseline of the chord beside it and touches it. Why: the sign and the chord root read as 1 smudge.
- Print the song title small, once, at the top right of every page. Why: a printed sheet is found in a stack by its corner.
- Set all text in SF Pro Display, installed from Apple on every machine that edits a score. Why: it carries every Vietnamese letter in all 4 styles.
- Make chords the biggest text with at least 0.75 chord font size of clear space between 2 chords. Why: chords are what gets read first.
#### Chords and signs
- Keep a chord baseline within 2.5 staff spaces of the staff top on a line with no notes. Why: the gate needs a stated distance, and the layout seats chords just above that.
- In a MuseScore 3 file, accept the fixed hold that program keeps after a mid-line barline, up to 1.7 staff spaces, and seat the rest at the barline. Why: its layout holds that chord there whatever the offset, and freeing it from automatic placement runs it into lyrics and voltas.
- Assign a chord to the bar holding the note or rest under it, never by a fixed distance from the barline; a chord with no event under it whose name crosses the barline belongs to the next bar. Why: a long name can start left of the barline it serves.
- Place each chord on its true beat: a lone chord in a bar without notes on beat 1, several chords in a bar without notes on real beats by their order, a lone chord in a bar with notes on beat 1 unless the source prints it exactly over a note in the 2nd half, and several chords in a bar with notes on the notes the source aligns them to. Why: a chord that looks right and plays late sounds wrong.
- Print every beat-1 chord with its left edge exactly at the barline, no gap, with or without notes, and right after the clef and key on a line start; left-align a mid-bar chord to its beat. Why: the same position everywhere reads at a glance, and centred chords shift with their width.
- Seat chords close above the staff, about 1 staff space, on a shared baseline per line that clears the highest note. Why: a chord reads as belonging to the bar it sits on.
- Judge a chord's height on a line with no notes from the highest mark under the chord row, a volta bracket or hairpin, else the staff top. Why: the line's 1 shared baseline rises over that mark by right.
- Print every chord in its shortest form that plays the same as its long form: `maj7` as `Δ7`, `maj9` as `Δ9`, `add2` and `add9` as `2`, `sus4` as `sus`, `m7(b5)` as `ø` (`ø7` in `-ms3` files), `dim` as `°`, `dim7` as `°7`, `7(b9)` as `7♭9`, `m7(add4)` as `m7add4`; other forms stay. Why: short names read faster and leave more room on a line.
- Require identical MuseScore playback between a chord's long and short forms on every MuseScore file built, with printing through a custom chord file; a form that cannot match keeps its long name. Why: a chord that prints right must also sound right.
- Write segno and coda as the score application's own built-in signs, enlarged through its own size setting. Why: a raw character behaves as plain text.
- Print segno and coda signs at least 1.6 times the chord size, each in an outlined box bolder than the source's, red for segno and blue for coda, in the chord row, on every sheet. Why: a performer must spot a jump at a glance; the signs are the only exception to chords being biggest.
- Print no jump words: a jump is the same boxed sign as its target, at the end of its bar, nearer its own line than the line above, beside any other sign in that bar, which widens to hold both, and never over a chord; a target opens its line at the line's left edge, before the clef. Why: the sign alone reads faster, and it stays on the bar it belongs to.
- Keep every jump a real jump in every MuseScore file built. Why: playback must still follow it.
#### Layout
- Print no empty staff on a line that shares it with another staff, in any file, including MuseScore 3's first line; a cue on a staff with no notes in that bar moves to the staff above. Why: an empty staff takes height and looks like missing music.
- Take as many pages as the song needs, on every sheet; a page count is never a limit. Why: readability and consistency outrank page turns.
- Lay out lines for reading, independent of the source and with no bars-per-line count: a section mark or a verse, pre-chorus, chorus or end label starts a line; no line starts on a chordless bar or breaks on a tied note where avoidable; no stranded single bar; each line fills until the next bar does not fit; between equal options, break at the chord change nearest a phrase boundary. Why: a count knows nothing about the music and forces bad breaks.
- Scale each sheet to the largest notation up to a size of 2.0 whose lines fill at least 85% of every page they need, with at most 15% of lines holding 2 bars or fewer, lines spread evenly, and a short line first taking a bar from the line above it. Why: bigger reads better, blank space serves nothing, and the cap keeps songs alike.
- WHEN no size holds a song to 15% short lines, allow up to 30% for it and list the song. Why: bigger notes outrank fuller lines.

#### Source mistakes
- Correct a mistake in a source sheet wherever evidence inside the song settles the right notation: a repeated section, the harmony, the bar length, the sibling verse. Why: the output must be musically correct, not a copy of an error.
- List every correction per song with bar, source reading, corrected reading and evidence. Why: each correction stays checkable against the source.
- Flag a suspected mistake in that list, unchanged, when the song holds no evidence that settles it. Why: a guess presented as a correction is a new error.

- Put readability ahead of tightness: readable notation size, clear gaps between lines, small margins, lines spread evenly across pages, no ink at the page edge. Why: the sheet is read while performing.

## Print mode
- Build in print mode while the owner needs every song printed for a rehearsal, and leave it the moment the owner asks for the strict rules again; trigger to restore: the owner says so. Why: polish rules were refusing sheets a musician reads without trouble.
- In print mode, loosen only these: a beat-1 chord within 2 points of its barline, 0.5 chord sizes of clear space between 2 chords, no page-fill floor, at most 40% of lines holding 2 bars or fewer, empty bars of a line within 4 points of each other, and a bar whose own chords crowd may widen to hold them, leaving the even-bars judgement when it holds 2 or more chords spaced by beat or its chords leave a gap within the even-bars tolerance of the clear chord gap. Why: none of them changes what a musician reads or plays.
- Keep every other rule strict in print mode: notes, chords, keys, jumps, repeats, boxed signs, chords in the right bar, no text on text, nothing past the page, piano kept bars, no lone bar. Why: these decide whether the sheet is right.
- Key every build on the mode, so a sheet built in print mode is never reused as strict. Why: the strict rules return unchanged.

## NEVER
- Never print a composer or performer credit, copyright line, contact, watermark, header, footer or page number. Cost: the sheet stops being music only.

## Piano PDF
- Print 1 staff per line, in the favourite tone. Why: a pianist voices from chords.
- Keep notes in the intro, interlude and outro, in every bar holding an accent, and from any bar whose cue contains fill or tutti through the following bars until the next chord change; every other bar is empty with chords above and its rest hidden. Why: fills and accented hits are what the pianist must play with, and other notes are not the pianist's concern.
- Keep a whole bar when any note in it is accented. Why: the notes around a hit place it.
- Treat bars before the first verse or section mark as the intro, a labelled interlude or an instrumental stretch without lyric labels as an interlude, and bars after the last chorus section as the outro; list each inferred boundary per song. Why: many sheets do not label these parts.
- Treat a coda without a lyric label as sung until the unbroken run of note bars that ends the song, which is the outro; treat a shorter coda stretch before a lyric label, and any lone kept bar inside the song, as sung. Why: isolated hits belong to the band, while the closing run is the pianist's.
- Treat an ending lyric that repeats an earlier section as sung for that section's length. Why: the singer still carries those bars.
- Treat a stretch after a repeat's end, cued for piano with no lyric label, as an interlude. Why: a piano-only passage there is the pianist's.
- In a kept bar whose chosen staff has no notes, take the notes of the other staff. Why: an instrumental passage can move between staves.
- In a kept part with 2 staves, keep the staff cued Piano or Key, otherwise the top staff; list each choice per song. Why: the staff the pianist plays can sit on either line.
- Never let chord text set a bar's width, on any sheet: bars with notes take width from their notes, and chords that do not fit put fewer bars on the line. Why: a bar's length shows time, not the length of a name.
- Make empty bars in a piano line equal width, leaving out a bar that carries a segno or coda sign. Why: even bars show the rhythm at a glance, and a sign's own room widens its bar by right.
- Join a short run of empty bars to the line above, even a line with notes, when the notes stay legible. Why: a line of 2 empty bars wastes height.
- Let a lyric label sit second on a piano line WHEN a section mark on the bar before opens that line. Why: starting both would strand the mark's bar alone.
- Avoid a lone bar on a piano line: move a neighbouring bar onto its line, even from a line of kept notes, unless the bar alone truly fills the line. Why: a stretched lone bar reads as wide and empty.
- Space 2 chords in an empty bar by beat position. Why: position shows the change without extra marks.
- Keep every cue, repeat, volta, jump sign, section mark, tempo and style; shorten each hint lyric to its first few words, per label where 1 text stacks several, and keep a bold cue sharing that text whole. Why: these are the map of the song and hint lines cost height.
- Size a piano sheet for density, the opposite of the band sheets: the size putting the most bars on each line and the fewest lines, never below the smallest readable size, with no lone bar and at most 15% of lines holding 2 bars or fewer, and no page limit. Why: a pianist voices from chords and wants as many as possible in 1 glance; a sparse piano sheet loses its point.
- Decide every layout question from the performer's own use of that sheet, never by asking the owner: a band player reads 1 part with room, a pianist scans chords. Why: the owner's expectation is the performer's common sense.
