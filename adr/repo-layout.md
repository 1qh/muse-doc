# Repo layout

Owns which repo holds what.

## MUST
- Keep only PDF and MuseScore files in `muse-sheets`, its history included. Why: the repo is the deliverable and nothing else.
- Keep correction lists, verdicts, pins and banned names in the untracked private folder of the workspace parent. Why: they quote sheet content and name people, and the other repos are public.
- Keep code in `muse`, docs in `muse-doc`, experiments in `muse-spike`, outputs in `muse-sheets`. Why: 1 home per kind.
- Keep source PDFs in the workspace parent, outside every repo. Why: the sheets are borrowed and not ours to publish.

- Publish only through `muse.publish`, which refuses a non-private repo or a failed gate, then purges replaced binaries from `muse-sheets` history with BFG and force-pushes, comparing the HEAD tree id before and after. Why: re-exported PDFs and scores otherwise accumulate on the remote.

- Keep song titles and reading pins in the private sheets repo or beside the sources. Why: public repos carry no song list.

## NEVER
- Never commit a source PDF, rendered page, or transcribed score to a public repo. Cost: republishes third-party work.
