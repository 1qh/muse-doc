# MuseScore fork

Owns the owner's own MuseScore build: what it adds to upstream, where it is built, and how it is released.

## MUST
#### What the fork is
- Keep `1qh/MuseScore` as a fork of `musescore/MuseScore` whose release branch is upstream's newest release tag plus a small set of patches, each 1 file under `patches/`. Why: a patch set rebases onto each release, while a diverged branch drifts.
- Carry exactly these patches: `serve` (a `--serve` mode that runs 1 conversion job per line of standard input and stays running), `piano-playback` (the keyboard panel lights the notes sounding during playback, upstream pull request 34351, fitted to the release), `swift-interop` (the Quick Look extension compiles under the newest Swift), `packaging` (the packager signs with the owner's identity and notarizes with an API key), `debloat` (extra build options pass through, and the welcome dialog is off). Why: each answers a need upstream leaves open; nothing else differs from upstream.
- Build the leanest app that plays back with the keyboard panel lighting the notes, and that the pipeline can run: switch off, through upstream's own module options, learning content, tours, update checks, cloud accounts, the Muse Sounds store, test automation, plugin hosts, braille, and every import or export format but MusicXML and PDF; keep notation, playback, the Muse Sounds playback engine, the converter and extensions; and open with no welcome dialog (`debloat` patch). Why: the owner wants playback with the highlight and nothing else, and the pipeline needs only import, layout, extensions and PDF.
- Drop a patch the moment upstream ships its change. Why: a patch upstream already carries is a conflict waiting for the next release.
- Build at upstream's newest release tag, against the Qt version upstream's own macOS build names in `buildscripts/ci/macos/install_qt.sh`. Why: upstream's QML compiler output for 1 Qt version does not compile against another, so the Qt version is a hold upstream decides; trigger to revisit: every upstream release.

#### Where it is built and released
- Build, sign, notarize and release on the owner's development Mac only, for Apple Silicon only. Why: the owner develops on 1 Mac and pays nothing for a hosted builder.
- Sign with the owner's Developer ID Application certificate and notarize with the App Store Connect API key, both read from the owner's secrets tree, and staple the ticket to the disk image. Why: a notarized, stapled app opens on any of the owner's Macs with no warning, offline included.
- Publish each build as a GitHub Release of the fork, named after the upstream tag it carries. Why: the other Macs install from 1 download page.

## NEVER
- Never store a signing certificate or API key in the fork or in a hosted builder. Cost: the fork is public, and the owner chose to keep every credential on 1 Mac.
- Never release a build whose patches did not all apply cleanly. Cost: a half-patched app looks like ours and lacks a feature.

## Pairs with
- `pipeline.md` — the `--serve` mode is what the pipeline's MuseScore launches use.
