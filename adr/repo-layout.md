# Repo layout

Owns which repo holds what.

## MUST
#### The sheets repo
- Keep only PDF and MuseScore files in `muse-sheets`, its history included. Why: the repo is the deliverable and nothing else.
- Keep `muse-sheets` public. Why: the owner chose to share the deliverable.
- Name every song a publish brings in its commit message, `feat: publish <songs>`. Why: a reader scans the commit list for what arrived.
- Purge from the sheets repo's history only the PDF and MuseScore versions absent from the current sheets, by blob, and keep every current file in every commit that carries it. Why: history stays as light as the sheets themselves, and each commit's file list shows only the songs it changed.
- Publish only through `muse.publish`, per song: a song passing its gate publishes, a failing song keeps its last good published folder and is listed with the reason, and only a repo-wide problem such as a stray file refuses the run. Why: 1 failing song must not hold back the rest. It then purges replaced binaries from `muse-sheets` history with BFG and force-pushes, comparing the HEAD tree id before and after. Why: re-exported PDFs and scores otherwise accumulate on the remote.

#### Song folders
- Give each song 1 folder named `<name>/` holding `<name>-<original>.mscz`, `<name>-<original>.pdf`, `<name>-<favourite>.pdf` and `<name>-<favourite>-piano.pdf`. Why: every use of a song sits in 1 place.
- Give an orchestral score only `<name>-<original>.mscz` and `<name>-<original>.pdf`. Why: it has no favourite tone or piano sheet.
- Skip `<name>-<favourite>.pdf` when the original already is the favourite tone. Why: it would duplicate `<name>-<original>.pdf`.
- Keep MuseScore files in the original tone only. Why: the favourite tone is delivered as print.

#### Homes
- Keep correction lists, verdicts, pins and banned names in the private `muse-spike`. Why: they quote sheet content and name people.
- Keep code in `muse`, docs in `muse-doc`, experiments in `muse-spike`, outputs in `muse-sheets`. Why: 1 home per kind.
- Keep source PDFs in the workspace parent, outside every repo. Why: the sheets are borrowed and not ours to publish.
- Keep song titles and reading pins in the private `muse-spike`, never in a public repo. Why: the public repos carry no sheet content.

## NEVER
- Never commit a source PDF, rendered page, or transcribed score to a public repo. Cost: republishes third-party work.
