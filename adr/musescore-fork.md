# MuseScore fork

Owns the owner's own MuseScore build: what it adds to upstream, where it is built, and how it is released.

## MUST
#### What the fork is
- Keep `1qh/MuseScore` as a fork of `musescore/MuseScore` whose release branch is upstream's newest release tag plus a small set of patches, each 1 file under `patches/`. Why: a patch set rebases onto each release, while a diverged branch drifts.
- Carry exactly these patches: `serve` (a `--serve` mode that runs 1 conversion job per line of standard input and stays running), `piano-playback` (the keyboard panel lights the notes sounding during playback, upstream pull request 34351, fitted to the release), `packaging` (the packager signs with the owner's identity and notarizes with an API key), `debloat` (extra build options pass through; no welcome dialog, tours, learning or account; the top bar holds Home and Score only; the menus carry no cloud save, online publishing, plugins, diagnostics, update check or online help, and Help keeps About and the factory reset; the home page shows no online scores or score manager; a save goes to the computer without asking; preferences show no cloud save, publishing, video, MIDI mapping or braille page; the app carries no translations, plugins folder or test-automation scripts; the app opens in the High-contrast Black theme with a grey accent, white text and a near-black border, score colours inverted, and the piano keyboard panel open). Why: each answers a need upstream leaves open; nothing else differs from upstream.
- Build the leanest app that plays back with the keyboard panel lighting the notes, and that the pipeline can run: switch off, through upstream's own module options, learning content, tours, update checks, cloud accounts, the Muse Sounds store, test automation, plugin hosts, the Finder Quick Look extension, translations, and every import or export format but MusicXML and PDF; switch off the Muse Sounds engine and its online sounds, so playback uses the built-in soundfont; keep notation, playback, the converter and extensions, and compiled but hidden every module whose QML another module imports (braille, learning, tours, cloud, the Muse Sounds store), and hidden the online-sounds status and preferences; and open with no welcome dialog (`debloat` patch). Why: the owner wants playback with the highlight and nothing else, and the pipeline needs only import, layout, extensions and PDF.
- Name the app MuseScoreLite with its own bundle identifier, and keep upstream's machine-readable name. Why: the owner tells it apart from upstream's app at a glance, its settings start fresh with the fork's defaults, and the data and extension folders the pipeline uses stay where upstream puts them.
- Drop a patch the moment upstream ships its change. Why: a patch upstream already carries is a conflict waiting for the next release.
- Build at upstream's newest release tag, against the Qt version upstream's own macOS build names in `buildscripts/ci/macos/install_qt.sh`. Why: upstream's QML compiler output for 1 Qt version does not compile against another, so the Qt version is a hold upstream decides; trigger to revisit: every upstream release.

#### Where it is built and released
- Compile on the owner's idle second Mac, with `release.sh` in build-only mode, and bring the built app back to the development Mac, for Apple Silicon only. Why: the compile is the heavy step, and the idle Mac takes it off the machine running the pipeline; no hosted builder costs anything.
- Name every module option the lean build changes or keeps, and each option derived from it such as its QML, ON or OFF, on every build. Why: CMake keeps a cached value, so dropping an option leaves its last setting in force.
- Launch the built app on a score before packaging, and refuse it when its log reports the main window failed to load. Why: a module switched off while another module's QML imports it leaves the app running with no window, and the build, the command line and the pipeline all pass.
- Rebuild only when a patch differs from the copy the build tree carries, and otherwise compile on the existing tree. Why: a fresh clone and full compile costs most of an hour, and a changed patch is the only thing that makes the tree stale.
- Point the built app's library search at the packaging Mac's own Qt and dependencies before packaging. Why: the compile Mac's paths are baked in, and the packager bundles Qt by resolving them.
- Sign, notarize, package and release on the development Mac only. Why: the signing identity and API key stay on 1 Mac, so build-only mode asks for neither.
- Sign with the owner's Developer ID Application certificate and notarize with the App Store Connect API key, both read from the owner's secrets tree, and staple the ticket to the disk image. Why: a notarized, stapled app opens on any of the owner's Macs with no warning, offline included.
- Publish each build as a GitHub Release of the fork, named after the upstream tag it carries. Why: the other Macs install from 1 download page.

## NEVER
- Never store a signing certificate or API key in the fork or in a hosted builder. Cost: the fork is public, and the owner chose to keep every credential on 1 Mac.
- Never release a build whose patches did not all apply cleanly. Cost: a half-patched app looks like ours and lacks a feature.

## Pairs with
- `pipeline.md` — the `--serve` mode is what the pipeline's MuseScore launches use.
