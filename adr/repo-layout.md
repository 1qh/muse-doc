# Repo layout

Owns which repo holds what.

## MUST
- Keep only PDF and MuseScore files in `muse-sheets`, its history included. Why: the repo is the deliverable and nothing else.
- Give each song 1 folder named `<name>/` holding `<name>-<original>.mscz`, `<name>-<original>-ms3.mscz`, `<name>-<original>.pdf`, `<name>-<favourite>.pdf` and `<name>-<favourite>-piano.pdf`. Why: every use of a song sits in 1 place.
- Skip `<name>-<favourite>.pdf` when the original already is the favourite tone. Why: it would duplicate `<name>-<original>.pdf`.
- Keep MuseScore files in the original tone only. Why: the favourite tone is delivered as print.
- Keep correction lists, verdicts, pins and banned names in the private `muse-spike`. Why: they quote sheet content and name people.
- Keep code in `muse`, docs in `muse-doc`, experiments in `muse-spike`, outputs in `muse-sheets`. Why: 1 home per kind.
- Keep source PDFs in the workspace parent, outside every repo. Why: the sheets are borrowed and not ours to publish.

- Publish only through `muse.publish`, which refuses a non-private repo or a failed gate, then purges replaced binaries from `muse-sheets` history with BFG and force-pushes, comparing the HEAD tree id before and after. Why: re-exported PDFs and scores otherwise accumulate on the remote.

- Keep song titles and reading pins in the private `muse-spike`, never in a public repo. Why: the public repos carry no sheet content.

## NEVER
- Never commit a source PDF, rendered page, or transcribed score to a public repo. Cost: republishes third-party work.
