# LTEE variant calling

How many mutations did *Escherichia coli* accumulate over 50,000 generations
of the Long-Term Evolution Experiment (LTEE), and does the answer hold when the
variant filter is made stricter? This project calls variants in three clones
from population Ara-3 (after 5,000, 15,000 and 50,000 generations) and repeats
the final filter at four minimum read depths.

It is the example project of the Reproducible and Open Science lessons
(CBIO7501). The analysis steps follow the Data Carpentry lesson
[*Wrangling Genomics*](https://datacarpentry.github.io/wrangling-genomics/)
(CC BY 4.0).

Author: Hamad Alhadhrami ([ORCID 0009-0005-9757-758X](https://orcid.org/0009-0005-9757-758X))

## Publication

There is no publication of this project yet. The data and the biology come
from:

> Tenaillon O, et al. Tempo and mode of genome evolution in a
> 50,000-generation experiment. *Nature* 536, pages 165 to 170 (2016).
> <https://doi.org/10.1038/nature18959>

## Reproduce the results

You need [Pixi](https://pixi.prefix.dev/latest/), an internet connection, and
Linux or macOS. On Windows, use WSL2: `bwa`, `samtools` and `bcftools` have no
Windows builds.

```bash
git clone https://github.com/hamadkhaled/ltee-project.git
cd ltee-project
pixi run reproduce
```

`pixi run reproduce` installs the exact software versions recorded in
`pixi.lock`, downloads the raw data and checks its checksums, runs the
Snakemake workflow (`workflow/Snakefile`, settings in `config/config.yaml`),
and renders the report. A GitHub Action (`.github/workflows/reproduce.yml`)
runs it on a fresh clone at every push.

Other entry points (`pixi task list` shows them all):

- `pixi run run-nextflow`: the same pipeline written in Nextflow (results in `results/nextflow/`)
- `pixi run compare-workflows`: checks that Snakemake and Nextflow give identical variant calls
- `pixi run compare-to-shell`: checks the Snakemake calls against the original shell script

A container image with the same software can be built from
`containers/Dockerfile`:

```bash
docker build --platform linux/amd64 -f containers/Dockerfile --build-arg ENV_NAME=pipeline -t ltee-project:0.1.0 .
```

Image ID of the build used for the results:
`sha256:84a78beea9e2782880899a4ef737698b65f27b01031b8f3a4c41d49863f5a76c`

## Hardware

Measured on 2026-11-23 on a MacBook with Apple silicon:

| What | Size or time |
|------|--------------|
| Software environments (`.pixi/envs/`) | 3.8 GB (default 1.1 GB, pipeline 2.7 GB) |
| Raw data (`data/raw/`) | 333 MB |
| Results of `pixi run reproduce` | about 100 MB (about 1.1 GB if you also run the Nextflow and shell-script comparisons) |
| Snakemake workflow, all steps | 29.5 seconds on 4 cores |
| Peak memory | about 340 MB |

Any recent laptop is enough; no GPU is needed.

## Outputs

| Path | Contents |
|------|----------|
| `reports/ltee-report.html` | The report: reads, quality, variant counts and the depth sensitivity analysis |
| `results/qc/multiqc/` | MultiQC summary of the FastQC reports |
| `results/vcf/<sample>_final_variants.vcf` | Filtered variants at minimum depth 2 |
| `results/sensitivity/<sample>_d<depth>.vcf` | Variants filtered at minimum depths 2, 5, 10 and 15 |
| `results/stats/` | Alignment statistics (`samtools flagstat`) and variant statistics (`bcftools stats`) |
| `results/bam/` | Sorted, indexed alignments |

Expected numbers of filtered variants at minimum depth 2:

| Sample | Generation | Variants |
|--------|-----------|----------|
| SRR2589044 | 5,000 | 10 |
| SRR2584863 | 15,000 | 25 |
| SRR2584866 | 50,000 | 775 |

## Data

- Raw reads: a trimmed and subsampled set of three runs prepared by Data
  Carpentry, on figshare, DOI
  [10.6084/m9.figshare.7726454.v3](https://doi.org/10.6084/m9.figshare.7726454.v3),
  license CC0. The original runs are in NCBI BioProject
  [PRJNA295606](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA295606).
- Reference genome: *E. coli* REL606, NCBI assembly GCA_000017985.1
  (GenBank CP000819.1).

The data is not stored in this repository: `scripts/download_data.py`
(`pixi run download`) downloads it. The sources, the providers' checksums and
our own checksums are in `metadata/`.

## Repository contents

See [`docs/structure.md`](docs/structure.md) for what each folder holds and its rules.

## Cite

See `CITATION.cff`; GitHub shows a "Cite this repository" button for it.

## License

- Code: MIT License, see `LICENSE`.
- Report text and figures: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- Raw data: CC0, from its provider (see Data).

## Contact

For questions or problems, open an issue on this repository. To contribute,
see `CONTRIBUTING.md`; everyone taking part follows `CODE_OF_CONDUCT.md`.

---
Project layout generated from the CBIO7501 project template
(<https://github.com/khan-lab/cbio7501-f26/tree/main/templates/cookiecutter-repro-project>).
