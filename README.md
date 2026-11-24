# LTEE variant calling

[![DOI](https://zenodo.org/badge/1437148089.svg)](https://doi.org/10.5281/zenodo.24511682)

Archived release v1.0.0: <https://doi.org/10.5281/zenodo.24511683>

Variant calling on three *E. coli* clones from population Ara-3 of Richard
Lenski's Long-Term Evolution Experiment (LTEE), frozen at 5,000, 15,000 and
50,000 generations. Question: how many mutations accumulated, and is the
answer robust to the minimum read depth used to filter variant calls?

Built as the example project of the Reproducible and Open Science lessons
(CBIO7501, Fall 2026). The analysis steps follow the Data Carpentry lesson
*Wrangling Genomics* (CC BY 4.0).

Author: keqin peng (ORCID [0009-0000-1487-1227](https://orcid.org/0009-0000-1487-1227))

## Publication

None yet. The data and biology are from Tenaillon et al. (2016), *Nature*
536:165–170, <https://doi.org/10.1038/nature18959>.

## Reproduce the results

Requirements: [Pixi](https://pixi.prefix.dev/latest/) and an internet
connection. Linux or macOS (on Windows use WSL2: `bwa`, `samtools` and
`bcftools` have no Windows builds). No other installation is needed.

```bash
git clone https://github.com/callmepikachu/ltee-project.git
cd ltee-project
pixi run reproduce
```

`pixi run reproduce` installs the exact software versions in `pixi.lock`,
downloads the raw data and checks its checksums, runs the Snakemake workflow
and renders the report. A GitHub Action runs the same command on every push.

Other entry points: `pixi run run-nextflow` (the same pipeline in Nextflow),
`pixi run compare-workflows` (checks that both give identical variant calls),
`pixi task list` (all tasks).

### Container

A Docker image of the pipeline environment is built from `pixi.lock`:
`docker build --platform linux/amd64 -f containers/Dockerfile --build-arg ENV_NAME=pipeline -t ltee-project:0.1.0 .`

Run the pipeline in it with the project folder mounted:
`docker run --rm --platform linux/amd64 -v "$PWD":/work -w /work ltee-project:0.1.0 bash scripts/run_variant_calling.sh`

Image ID of the image that produced the container results (2026-11-17):
`sha256:a052a76234257f6bd8485d62eea8cab22da1cf4db5fa4ff0bb6103a6532e5c66`

## Hardware

Any laptop from the last few years. Measured on 2026-11-24 (Apple Silicon Mac,
macOS, 4 cores used): about 3.7 GB of disk for the software environments,
440 MB for the raw data (including the 110 MB download archive) and 100 MB for
the results; the Snakemake workflow takes about 25 seconds and less than
400 MB of memory.

## Outputs

| Path | Content |
|------|---------|
| `reports/ltee-report.html` | The report: data, read quality, variants, the depth sensitivity analysis, software versions |
| `results/qc/multiqc/multiqc_report.html` | Read quality summary |
| `results/vcf/` | Filtered variant calls per sample (minimum depth from `config/config.yaml`) |
| `results/sensitivity/` | Variant calls at each depth in `sensitivity_depths` |
| `results/stats/` | Alignment (`flagstat`) and variant (`bcftools stats`) statistics |

Expected filtered variants at minimum depth 2: SRR2589044 10, SRR2584863 25,
SRR2584866 775.

## Data

Raw reads: a subset of three runs prepared by Data Carpentry, archived at
figshare, <https://doi.org/10.6084/m9.figshare.7726454> (file `sub.tar.gz`,
CC0). The original sequencing runs are in NCBI BioProject PRJNA295606
(SRR2589044, SRR2584863, SRR2584866). Reference genome: *E. coli* B str.
REL606, GenBank CP000819.1 (assembly GCA_000017985.1, NCBI).

Source URLs, provider checksums, SHA-256 checksums and the sample sheet are in
`metadata/`. Raw files are not stored in this repository: download them with
`pixi exec python scripts/download_data.py` and verify them with
`sha256sum -c metadata/checksums.sha256`. Derived data (`data/processed/`) and
results (`results/`) are rebuilt from the raw data by the project code and are
not archived separately.

## Repository contents

See the table in `docs/structure.md` (each folder also has its own README).

## Cite

See `CITATION.cff` (GitHub: "Cite this repository").

## License

Code: MIT (`LICENSE`). Report text and figures: CC BY 4.0. The raw data is
CC0 (see Data).

## Contact

Questions and problems: open an issue on GitHub. Contributions: see
`CONTRIBUTING.md`. Everyone taking part follows `CODE_OF_CONDUCT.md`.
