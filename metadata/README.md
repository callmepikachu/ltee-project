# metadata

Small text files that describe the data. They are committed to Git.

## Files

- `sources.tsv`: where each raw file is downloaded from.
  - `name`: short label of the download
  - `url`: download address
  - `md5`: checksum published by the provider (copied 2026-10-24)
  - `unpack`: `tar` if the download is an archive to unpack, `none` if it is kept as is
  - `dest`: where the file or its contents go in the project
- `checksums.sha256`: SHA-256 of every raw file we keep. Check with
  `sha256sum -c metadata/checksums.sha256`
  (macOS before 14: `shasum -a 256 -c metadata/checksums.sha256`).
- `samples.tsv`: one row per sample.
  - `sample`: SRA run accession
  - `strain`: LTEE clone name (REL...)
  - `generation`: generation at which the clone was isolated
  - `population`: LTEE population (Ara-3)
  - `mutator`: `None` or `plus` (hypermutator phenotype)
  - `cit`: `plus` or `unknown` (citrate-use phenotype)
  - `fastq_1`, `fastq_2`: paths to the paired reads, relative to the project folder

## Source

Sample metadata adapted from Data Carpentry, Wrangling Genomics (CC BY 4.0),
which draws on Tenaillon et al. 2016, Nature 536:165, doi:10.1038/nature18959.
The sequencing runs are in BioProject PRJNA295606.
