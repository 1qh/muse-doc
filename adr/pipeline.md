# Pipeline

Owns how a source PDF becomes a final sheet. The reader row is provisional until the comparison in `muse-spike` measures it.

| Stage | Pick | State | Disproved by |
|---|---|---|---|
| Read notes | Direct reader of PDF symbols, cross-checked by homr | provisional | a bar disagreeing with the source when read by eye |
| Read chords and text | Direct PDF text extraction | provisional | a chord or cue missing from the output |
| Transpose | MuseScore `--transpose` | provisional | a wrong chord extension on a real sheet |
| Export | MuseScore `mscore` batch job | provisional | a missing or altered element in the export |
