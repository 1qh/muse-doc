# Practice video

Owns the owner's practice video of a song: a 4K picture of the 61-key keyboard above the current and next staff line with the app's playback cursor, and the song's sound on the owner's 3 presets with a metronome, rendered offline from the published favourite-key sheet.

## MUST
#### Source
- Render each song's favourite-key sheet from the chosen size's final score through `render.Batcher`, with the song's own transpose options from `port.transpose_options` when the favourite key differs from the original, writing the PDF, MIDI, `.spos` and `.mpos` in 1 job. Why: the work score is stored untransposed and only that render prints the published key; 1 job keeps picture, cursor and sound on 1 load of the sheet.
- Refuse a song whose rendered PDF is not pixel-identical to the published favourite-key PDF. Why: the video must show exactly the published sheet.
- Read each MIDI channel's role from the score's own channel declarations, in declaration order: a channel named `harmony` is the chord track, every other channel its part's staff. Why: the channels carry neither names nor distinct programs, and removing the chord symbols empties exactly the `harmony` channel on every song.

#### Sound
- Edit the MIDI only in ticks with `mido`: on each channel a bank select before its program change, the chord track on DynamicFullPad (`PianoLayer.sf2`, bank 22, program 8), every staff on OrchestraRound (`PianoLayer.sf2`, bank 22, program 28), a woodblock (WoodblockRound, `Percussion.sf2`, bank 21, program 52) on every beat of the time signature with the downbeat accented, and everything shifted by 1 bar of count-in. Why: MuseScore writes its tempo map on a later track, and a bank select after the program falls back to bank 0.
- Render the sound with FluidSynth at 48 kHz from the owner's soundfonts, and refuse a channel that resolves no preset. Why: FluidSynth is the engine those soundfonts play through in the app.

#### Picture
- Draw 3840 by 2160 at 30 frames a second: the 61 keys from C2 to C7 across the top with the C keys labelled, each sounding key lit in its layer's palette colour with staff 1 above staff 2 above the chord track; below, the current line above the next, each cropped from the PDF between the quietest ink rows that separate it from its neighbours and inverted to the app's High-contrast Black look. Why: these are the app's own keyboard and score as the owner practises with them.
- Draw the app's own playback cursor: voice 1's selection colour `#0065BF` at alpha 178, 0.4 spaces wide, 1 space left of the position interpolated by time between the `.spos` segments, from 3 spaces above the line's first staff to 3 spaces below its last. Why: the owner reads the cursor they see in the app.
- Advance the pair only when the cursor reaches another line, so the current line always sits above the next and repeats and jumps follow the playback order. Why: the picture never moves except where the music goes.
- Show the first line pair without a cursor during the count-in, and hold the last frame 2 s after the last note ends. Why: the count-in prepares the first bar, and the end settles.

#### Output
- Encode the frames with `ffmpeg`'s VideoToolbox HEVC encoder (tag `hvc1`) and the sound as AAC into `<videos>/<song>-<key>.mp4`, outside every repository. Why: the hardware encoder renders a 4-minute song in about 2 minutes, and the videos are the owner's practice material, not a published deliverable.

## NEVER
- Never render the video's sheet by hand outside the pipeline's render path. Cost: a score rendered without its style, chord file or transpose options prints a different layout or key.
- Never select a preset by name alone. Cost: `Strings.sf2` also carries a FullOrchestraRound.

## Dependencies
- mido: reads and writes the MIDI in ticks across every track, keeping the tempo map MuseScore writes off track 0. Its newest release is older than the maintenance window while its repository stays active, and pretty_midi, released inside the window, builds on it; logged exception, trigger to revisit: the repository archived or a release of pretty_midi that keeps a tempo map off track 0. Rejected: pretty_midi, which drops that tempo map; symusic and miditoolkit, outside the window.
- FluidSynth and ffmpeg: installed by Homebrew on the compile Mac, both active.

## Pairs with
- `pipeline.md` — the render path and the sheet this video shows.
