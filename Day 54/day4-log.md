# Day 4 Log — Road Accident Insight Explorer

**Date:** August 4, 2026
**Phase:** Implementation — Data Cleaning & ETL Pipeline

## What was done
- Built `src/data_cleaning.py`: full ETL pipeline reading the raw UK Road Safety CSV and producing a cleaned Parquet file
- Implemented STATS19 lookup tables to decode numeric severity/weather/road-type codes into readable labels
- Parsed dates and derived `hour`, `day_of_week`, `month`
- Handled missing values (dropped only rows missing truly critical fields; filled non-critical gaps)
- Filtered invalid coordinates and negative casualty counts
- Ran the pipeline end-to-end: 117,536 → 117,508 rows (99.98% retention)
- Verified output against `docs/SCHEMA.md` — all 13 columns present, correct types, no unexpected nulls
- Committed and pushed Day 3 + Day 4 work together (Day 3's `config.py` and exploration script hadn't been committed yet)
- Fixed a `.gitignore` gap: raw dataset CSV was accidentally committed once, then correctly removed from tracking

## Decisions made
- Kept `time`/`hour` nullable per schema design (63 rows lack an exact reported time) rather than dropping or fabricating values
- Chose Parquet over CSV for the processed file — 3.0 MB vs. 15 MB raw, consistent with the Day 2 architecture decision

## Blockers encountered (all resolved)
- VS Code auto-closing quotes corrupted a large pasted code block — fixed by disabling `editor.autoClosingQuotes`/`editor.autoClosingBrackets`
- Raw CSV briefly got committed before the `.gitignore` rule was in place — cleaned up with `git rm --cached`

## Day 5 readiness
Cleaned dataset ready at `data/processed/cleaned_accidents.parquet`. No further data cleaning needed. Day 5 starts directly on `src/insights.py` — the analytics and plain-language insights engine.
