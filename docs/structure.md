# Project structure

| Folder | Contents | Rule |
|--------|----------|------|
| `data/raw/` | Data exactly as downloaded | Downloaded by `scripts/download_data.py`. Read-only. Not committed. |
| `data/processed/` | Data derived from the raw data | Rebuilt by the workflow. Not committed. |
| `metadata/` | Sample sheet, data sources, checksums, data dictionary | Small text files, committed. |
| `scripts/` | Code we write (Python, shell) | Committed. |
| `config/` | Settings for the analysis, shared by Snakemake and Nextflow | Committed. |
| `workflow/` | Snakemake (`Snakefile`) and Nextflow (`nextflow/`) pipelines | Committed. |
| `notebooks/` | Jupyter notebooks for exploration | Committed, outputs cleared. |
| `reports/` | Quarto report | Source committed, rendered HTML and PDF not. |
| `docs/` | Longer documentation: this file, decisions, software versions | Committed. |
| `results/` | Figures, tables, alignments and variant calls made by the code | Rebuilt. Not committed. |
| `envs/` | Conda `environment.yml` exports, for people without Pixi | Generated from `pixi.toml`. |
| `containers/` | Dockerfile and Apptainer definition, built from `pixi.lock` | Committed. |
| `.github/workflows/` | The reproduce-from-a-clean-clone check | Committed. |
