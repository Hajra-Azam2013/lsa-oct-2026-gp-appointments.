# LSA Oct 2026: appointments data pipeline

## What it does
A four-stage pipeline over three NHS appointment files:
extract (copy raw files, convert Excel to CSV, write a manifest and data profile),
transform (clean and write Parquet), load (DuckDB), checks (data quality).
Only the extract stage is built so far (Week 1).

## Data source credit
[ADD: name of the data publisher, dataset title and link from your course materials]

The data files are NOT in this repository. `manifest.json` records exactly which
files were used (name, size, SHA-256, row count, time extracted).

## How to run
```bash
pip install -r requirements.txt
python run_pipeline.py --src /path/to/folder/with/the/three/source/files
pytest
```
Source files expected in that folder: `actual_duration_csv.xlsx`,
`appointments_regional.csv`, `national_categories.csv`.
To verify your copy matches, compare SHA-256 values with `manifest.json`.

## Layout
| Path | What |
|---|---|
| `src/extract.py`, `transform.py`, `load.py`, `checks.py` | the four pipeline stages |
| `run_pipeline.py` | runs all four in order |
| `tests/` | pytest tests |
| `docs/` | data profile (decision log and quality report added in later weeks) |
| `app/` | Week 4 Streamlit dashboard |
| `data/raw/`, `data/clean/`, `warehouse/` | generated locally, git-ignored |

## Week 1 findings (see docs/data_profile.md)
Five traps: Excel file named like a CSV; hidden BOM in the national file; day-first
dates (dd/MM/yyyy); 21,604 exact-duplicate regional rows that are real appointments
(sum, never drop); untidy labels (double spaces, case mismatch, band sort order).
