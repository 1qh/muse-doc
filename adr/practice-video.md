# Practice video

Owns which score each song's practice video is made from and how the pipeline asks MuseScoreLite to make it; everything the video shows and sounds is the app's own, owned by `mslite-doc/adr/lean-build.md`.

## MUST
- Export each song's practice video from the favourite-key sheet's own final score, `<work>/<song>-<variant>-<size>-final.mscz` named by the song's build record, with the song's transpose options from `port.transpose_options` when the favourite sheet is the transposed one. Why: the work score is stored untransposed at the size its own sheet chose, so only that score with those options paints the published favourite-key sheet.
- Export a second video in the original key, from the original sheet's own final score with no transpose, for every song whose favourite-key video is transposed. Why: the owner also practises the song as written, and a song already in the favourite key needs only 1.
- Write every song's export as 1 entry of 1 MuseScore job file and run it once through `mscore --view-mode practice -j`, with `MUSE_MSCORE` naming the app. Why: 1 launch loads the app once for every song, and the job file already carries a per-entry transpose.
- Name each video `<song>-<key>.mp4` by the key it sounds in, in a folder outside every repository. Why: the videos are the owner's practice material, never a published deliverable.
- Refuse a song whose final score is absent, naming the path. Why: a video made from another score shows another sheet.

## NEVER
- Never draw, mix or encode any part of the video in the pipeline. Cost: a second drawing of the keyboard, the score or the cursor drifts from the app the owner practises with.

## Pairs with
- `pipeline.md` — the build record and the transpose options the video reads.
- `mslite-doc/adr/lean-build.md` — the practice view mode itself.
