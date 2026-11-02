# Pipeline

Owns how a source PDF becomes a final sheet. The reader row is provisional until the comparison in `muse-spike` measures it.

| Stage | Pick | State | Disproved by |
|---|---|---|---|
| Read | PDFtoMusic Pro, Audiveris, homr or a custom vector reader | provisional | note-by-note score on 1 source page |
| Transpose | MuseScore `--transpose` | provisional | a wrong chord extension on a real sheet |
| Export | MuseScore `mscore` batch job | provisional | a missing or altered element in the export |
