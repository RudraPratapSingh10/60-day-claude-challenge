# Project Structure — Road Accident Insight Explorer

**Status:** Approved Day 2 design — this is the confirmed structure; Day 3 setup will not change it.

```
road-accident-explorer/
├── data/
│   ├── raw/                  # original downloaded dataset, untouched (Day 3)
│   │   └── .gitkeep
│   └── processed/            # cleaned dataset the live app reads (Day 4)
│       └── .gitkeep          # will hold cleaned_accidents.parquet
├── src/
│   ├── data_cleaning.py      # offline ETL: raw CSV -> cleaned Parquet (Day 4)
│   ├── insights.py           # plain-language insight generation logic (Day 5)
│   └── app.py                # main Streamlit app: filters, charts, map (Days 5-7)
├── notebooks/                # exploratory analysis, not shipped to production
│   └── .gitkeep
├── docs/                     # design documentation (this folder, Day 2 deliverables)
│   ├── ARCHITECTURE.md
│   ├── SCHEMA.md
│   ├── API.md
│   ├── UI-WIREFRAMES.md
│   └── PROJECT-STRUCTURE.md
├── requirements.txt          # exact pinned dependencies (filled in Day 3)
├── README.md                 # project overview, setup steps, live link (finalized Day 10)
└── .gitignore                # Python template, already in place
```

## Folder responsibilities

| Folder/file | Responsible for | Future code lives here |
|---|---|---|
| `data/raw/` | The untouched, original dataset as downloaded (Day 3) | No code — data only |
| `data/processed/` | The single cleaned Parquet file the live app reads at runtime | No code — data only |
| `src/data_cleaning.py` | All ETL logic: parsing, standardizing categories, deriving `hour`/`day_of_week`/`month`, validation (per `SCHEMA.md`) | `load_raw_data()`, `clean_data()`, `save_processed_data()` |
| `src/insights.py` | All plain-language insight generation logic (per `API.md`) | `generate_insights()` and its sub-functions |
| `src/app.py` | The Streamlit UI: filter widgets, chart rendering, page layout (per `UI-WIREFRAMES.md`) | `load_cleaned_data()`, `apply_filters()`, `render_map()`, `render_trend_charts()`, `render_severity_chart()` |
| `notebooks/` | Scratch space for exploring the dataset before committing to cleaning rules — optional, never imported by `src/` | Jupyter notebooks only, not shipped |
| `docs/` | All Day 2 design documents, kept in the repo so any future AI session or collaborator can read the source of truth without re-deriving it | Documentation only |
| `requirements.txt` | Exact dependency versions for reproducible installs and deployment | N/A |
| `README.md` | Human-facing project overview; finalized on Day 10 | N/A |

## Why this structure

- **`data/` vs `src/` separation** keeps data artifacts (which can be large and shouldn't be diffed like code) clearly apart from logic.
- **Three files in `src/`, not one big `app.py`** — this maps directly to the three responsibilities defined in `API.md` (ETL, insights, UI), so each day's Blueprint work (Day 4 = cleaning, Day 5 = insights, Days 6–7 = UI) touches exactly one file, minimizing merge/context-switching overhead in a solo project.
- **`notebooks/` is explicitly excluded from the shipped app** — exploratory work never becomes a hidden dependency of the production app, avoiding a common "works on my machine, breaks on deploy" failure mode noted in the Blueprint's Day 9 debugging tips.
- **`docs/` added in addition to the original Blueprint folder list** — this is a small, additive change: the Blueprint didn't originally list a `docs/` folder, but today's deliverables need a home in the repo. This doesn't conflict with any approved decision, it just gives the 5 new files a place to live.

## Confirmation against Blueprint Day 2

This matches the Blueprint's Day 2 folder structure exactly, with one addition (`docs/`) to house today's deliverables. No other deviation.
