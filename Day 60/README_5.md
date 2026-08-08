# Road Accident Insight Explorer

**Live app:** https://road-accident-explorer-auna7myd9rnabtzduzheru.streamlit.app

![Version](https://img.shields.io/badge/version-1.0.0-blue) ![License](https://img.shields.io/badge/license-MIT-green) ![Python](https://img.shields.io/badge/python-3.13-blue) ![Streamlit](https://img.shields.io/badge/streamlit-1.60.0-red)

Interactive web app for exploring road accident data — filter by region, date, severity, and weather, and instantly see a hotspot map, trend charts, and auto-generated plain-language insights. No SQL, no Power BI license, no login required.

Built as a 10-day capstone project for the AB Talks 60-Day Claude AI Challenge — taken from a blank requirements doc to a publicly deployed v1.0.0 in 9 working days.

> *Add a screenshot or short GIF of the live app here — the hotspot map and insights panel together make the strongest first impression.*

## What it does

- **Filter** 117,508 real UK road accident records (2019) by region, date range, severity, and weather condition
- **See a hotspot map** — a density heatmap showing where accidents concentrate
- **See trend charts** — accidents by hour of day, day of week, and severity breakdown
- **Read auto-generated insights** — plain-language sentences summarizing patterns in your current filter selection, computed live from the data (no AI/LLM calls — pure statistics)

## Tech stack

- **Streamlit** — the entire app, UI and logic
- **pandas** — data loading, filtering, aggregation
- **Plotly** — every chart and the hotspot map (no separate mapping library or API key needed)
- **Parquet** — the cleaned dataset storage format
- No database, no authentication, no external APIs — fully free-tier, zero ongoing cost

## Data source

UK Department for Transport — Road Safety Data, Accidents 2019, released under the [Open Government Licence](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Region names decoded via the DfT's official Road Safety Open Data Guide.

## Running locally

See [`docs/SETUP.md`](docs/SETUP.md) for full setup instructions, or the short version:

```
git clone https://github.com/RudraPratapSingh10/road-accident-explorer.git
cd road-accident-explorer
python -m venv venv
.\venv\Scripts\Activate.ps1        # Windows
python -m pip install -r requirements.txt
streamlit run src/app.py
```

You'll also need the raw dataset (see `docs/SETUP.md` for the download link) placed at `data/raw/road_safety_accidents_2019.csv`.

## Project documentation

Full design and build documentation lives in [`docs/`](docs/):
- `ARCHITECTURE.md` — system design, data flow, request lifecycle
- `SCHEMA.md` — data schema and field definitions
- `API.md` — internal module interfaces
- `UI-WIREFRAMES.md` — UI design and user flow
- `PROJECT-STRUCTURE.md` — folder structure and rationale
- `SETUP.md` / `ENVIRONMENT.md` — full local setup guide

`TESTING.md` (project root) has the full QA test log and bug history. `challenge-retrospective.md`, `future-scope.md`, and `30-day-growth-plan.md` cover the project's build journey and what comes next.

## License

MIT — see [`LICENSE`](LICENSE). The underlying dataset is UK Crown copyright under the Open Government Licence v3.0 (see above).

---

Built with Claude as part of the AB Talks 60-Day Claude AI Challenge.
