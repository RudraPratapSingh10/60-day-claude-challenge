# API / Module Interface Design — Road Accident Insight Explorer

**Status:** Approved Day 2 design

## A note on what "API" means for this app

Streamlit apps are a single Python process with no client-server split — there is no REST/HTTP API for the browser to call, and building one would add complexity the PRD doesn't need (no external clients, no mobile app, no third-party integrations in v1.0). So instead of REST endpoints, this document specifies the **internal function contracts** between the three modules (`data_cleaning.py`, `app.py`, `insights.py`) — the same information a REST API spec would capture (purpose, inputs, outputs, validation, error handling), just at the function-call level instead of the HTTP level. This satisfies the intent of "no implementation yet, but the full interface is defined" without introducing an unnecessary architecture layer. No PRD or Blueprint scope changes as a result.

---

## `src/data_cleaning.py` — offline ETL (run once, not part of the live app)

### `load_raw_data(path: str) -> pd.DataFrame`
- **Purpose:** Read the raw dataset CSV into a DataFrame.
- **Input:** `path` — file path to raw CSV in `data/raw/`
- **Output:** Unprocessed pandas DataFrame
- **Validation:** File must exist and be non-empty; raises `FileNotFoundError` otherwise
- **Error cases:** Malformed CSV → pandas parsing error surfaced directly (this runs offline, so a stack trace is acceptable — a human is watching)

### `clean_data(df: pd.DataFrame) -> pd.DataFrame`
- **Purpose:** Apply all cleaning rules from `SCHEMA.md` (parse dates, standardize categories, derive `hour`/`day_of_week`/`month`, validate coordinates, deduplicate)
- **Input:** Raw DataFrame
- **Output:** Cleaned DataFrame matching the `cleaned_accidents` schema
- **Validation:** Asserts all required columns exist post-cleaning; asserts `severity` only contains the 3 allowed categories plus `Unknown`
- **Error cases:** Missing required source column → raises a clear `ValueError` naming the missing column (fail fast during Day 4, not silently at runtime)

### `save_processed_data(df: pd.DataFrame, path: str) -> None`
- **Purpose:** Write the cleaned DataFrame to Parquet (with CSV fallback) in `data/processed/`
- **Input:** Cleaned DataFrame, output path
- **Output:** None (writes file to disk)
- **Validation:** Confirms row count didn't unexpectedly drop to near-zero after cleaning (sanity check against silent data loss)

---

## `src/app.py` — live application (runs on every user session/interaction)

### `load_cleaned_data() -> pd.DataFrame`
- **Purpose:** Load the cleaned Parquet file into memory
- **Input:** None (path is a constant)
- **Output:** Cleaned DataFrame
- **Caching:** Decorated with `@st.cache_data` — loads from disk once per app session, not on every filter change
- **Auth:** None (public app)
- **Error cases:** File missing → user-facing message: *"Data file not found — please contact the app maintainer"* (should never occur in production; the file ships with the repo)

### `apply_filters(df: pd.DataFrame, filters: dict) -> pd.DataFrame`
- **Purpose:** Return the subset of rows matching the active filter panel state
- **Input:** `filters` dict — `{region: list, date_range: (start, end), severity: list, weather: list}`
- **Output:** Filtered DataFrame
- **Validation:** Empty filter lists are treated as "no filter applied" for that field (not "match nothing")
- **Error cases:** `date_range` start after end → swapped automatically rather than erroring, to stay forgiving of user input (NFR-1 usability)

### `render_map(filtered_df: pd.DataFrame) -> None`
- **Purpose:** Render the Plotly hotspot map for the current filter state
- **Input:** Filtered DataFrame
- **Output:** Renders directly via `st.plotly_chart`; no return value
- **Error cases:** If `filtered_df` has zero rows with valid coordinates → renders the "no data" state (NFR-3) instead of an empty/broken map

### `render_trend_charts(filtered_df: pd.DataFrame) -> None`
- **Purpose:** Render hour-of-day and day-of-week bar charts (FR-4)
- **Input:** Filtered DataFrame
- **Output:** Renders two `st.plotly_chart` calls
- **Error cases:** Zero-row DataFrame → "no data" state

### `render_severity_chart(filtered_df: pd.DataFrame) -> None`
- **Purpose:** Render severity distribution (FR-5)
- **Input:** Filtered DataFrame
- **Output:** Renders one `st.plotly_chart` call
- **Error cases:** Zero-row DataFrame → "no data" state

---

## `src/insights.py` — plain-language insight generation

### `generate_insights(filtered_df: pd.DataFrame) -> list[str]`
- **Purpose:** Orchestrates the individual insight functions below and returns 3–5 sentences for the insights panel (FR-6)
- **Input:** Filtered DataFrame
- **Output:** List of plain-language strings, e.g. `"62% of accidents in this selection occurred between 5–8 PM."`
- **Validation:** If `filtered_df` has fewer than a minimum row threshold (e.g. 5 rows), returns a single message: *"Not enough data in this selection to generate reliable insights — try widening your filters."* rather than misleading stats from a tiny sample
- **Error cases:** None expected — pure computation over an already-validated DataFrame

### `compute_time_insight(df: pd.DataFrame) -> str`
- **Purpose:** Identify and describe the peak accident time window
- **Input/Output:** DataFrame in, single sentence out

### `compute_severity_insight(df: pd.DataFrame) -> str`
- **Purpose:** Describe the proportion of severe/fatal accidents in the current selection
- **Input/Output:** DataFrame in, single sentence out

### `compute_weather_insight(df: pd.DataFrame) -> str`
- **Purpose:** Describe any notable weather-condition pattern (e.g. disproportionate share of accidents in rain)
- **Input/Output:** DataFrame in, single sentence out

---

## Authentication & authorization

Not applicable — no login, no user roles, no protected routes (per PRD scope exclusions).

## Rate limiting / abuse handling

Not applicable — no external API is exposed for others to call.
