# fp-datasources

A small, public, append-only store of biomedical datasets, hosted so they can be fetched anonymously and pinned by SHA at build time.

Each subdirectory is one dataset: the data files themselves (tracked via Git LFS so single files >100 MB are not blocked by GitHub's per-file Git cap) and a JSON provenance file per data file describing where it came from and how it was derived.

## Datasets

### `xena-toil-breast/`

Aggregated TCGA breast-cancer + GTEx healthy-tissue gene expression matrices, derived from the [UCSC Xena Toil RNA-seq recompute](https://xena.ucsc.edu/).

- `expression.parquet` — 156 MB — sha256 `39023c897a1a67db51b4f6b0d75000439a284d506cbafe42ded0b13fdbb51986`
- `samples.parquet` — 16 KB — sha256 `93b7d775a54cddf28de69259abcd720e811d6747c435f1674cdd826f05c31955`

Per-file provenance: `<file>.provenance.json` alongside each parquet (upstream URL + sha + bytes + derivation + attribution).

## Anonymous download URLs

Data files (LFS-resolved):
```
https://media.githubusercontent.com/media/craigfo/fp-datasources/main/<dataset>/<file>.parquet
```

Provenance files (plain raw):
```
https://raw.githubusercontent.com/craigfo/fp-datasources/main/<dataset>/<file>.parquet.provenance.json
```

Note: `raw.githubusercontent.com` serves the *LFS pointer text* (~130 bytes) for LFS-tracked files, not the binary. Use `media.githubusercontent.com/media/...` to fetch the resolved binary.

## Attribution (required by upstream licences)

These files are derivative aggregations of:

1. **UCSC Xena Toil RNA-seq recompute** — Vivian, J. et al. *Toil enables reproducible, open source, big biomedical data analyses.* Nature Biotechnology 35, 314–316 (2017). https://xena.ucsc.edu/
2. **TCGA** — distributed by the NCI Genomic Data Commons (GDC) under the NIH Genomic Data Sharing Policy. https://gdc.cancer.gov/
3. **GTEx** — open-access tissue gene expression distributed by the GTEx Portal under the NIH Genomic Data Sharing Policy. https://gtexportal.org/

## Conditions of use (inherited from upstream)

- **Acknowledge** UCSC Xena Toil, TCGA via the GDC, and GTEx via the GTEx Portal in any downstream paper, presentation, or derived dataset.
- **Do not attempt to re-identify** the de-identified donors in the GTEx or TCGA cohorts (NIH Genomic Data Sharing Policy).
- **Only Open Data tier** files are republished here. No dbGaP-Controlled material is in scope.

If any upstream's terms change, this repo's README should be updated to match. Re-verify on each new use.

## Repo design

- Data files are tracked via Git LFS (see `.gitattributes`).
- The git tree holds the README, the `.gitattributes`, and one JSON provenance file per data file. Nothing else.
- History is kept clean: single-commit. Updates to existing datasets replace the file + bump its provenance; new datasets land as a new subdirectory.
