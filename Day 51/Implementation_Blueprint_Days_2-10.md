# Implementation Blueprint — Days 2 to 10
## Road Accident Insight Explorer

**This document is the single source of truth for building this project.** Every remaining day begins in a fresh AI conversation — paste that day's section (plus the PRD) and the assistant should be able to continue building immediately, without re-planning or re-deciding architecture.

**Golden rule for every day:** protect the scope defined in the PRD. If a new idea comes up mid-build that isn't in v1.0 scope, write it under that day's "Parking Lot" note and move on.

---

## 📌 Project Snapshot (paste this into every new day's conversation)

- **Project:** Road Accident Insight Explorer — a deployed, interactive web app letting non-technical users explore road accident data via filters, a hotspot map, trend charts, and auto-generated plain-language insights.
- **Founder background:** B.Tech CSE, final year. Strong in SQL, Power BI, Excel, general data analysis. Basic Python. Has completed one prior self-placed project on road accident data + data warehousing.
- **Constraints:** No paid tools/services. Must be deployable for free. No login/accounts. No predictive ML in v1.0.
- **v1.0 scope:** dataset cleaning, filter panel (location, time, severity, weather), hotspot map, trend charts, severity breakdown, auto-generated insights panel, public deployment.
- **Out of scope for v1.0:** real-time data, predictive ML, accounts, native mobile, multi-language, multi-dataset merging.

---

# Day 2 — Design (Tech Stack Decision + Architecture)

### 🎯 Objective
Lock in the technical foundation: choose the tech stack, design the system architecture, define the data schema, and sketch the UI layout — so that from Day 3 onward, it's pure execution with zero architectural debate.

### 📖 What I'll learn
- How to evaluate a tech stack against skills, timeline, and deployment constraints (not just "what's popular")
- How to design a simple data flow architecture (raw data → cleaned data → app)
- How to translate feature requirements into a UI wireframe before writing code

### 🛠 Features to build
None yet — this is a planning/design day. No code is written today except possibly a throwaway proof-of-concept.

### 📝 Step-by-step implementation plan

1. **Tech stack decision (guided, not prescribed).** Compare 2–3 realistic options against the founder's skill profile (strong Python basics + SQL/Excel/Power BI, limited JS/frontend experience) and the free-hosting constraint. Recommended framing:
   - **Option A — Streamlit (Python-only):** Fastest path from data analysis skillset to a working interactive app. Built-in widgets for filters, native support for Plotly/Folium charts and maps, free deployment via Streamlit Community Cloud. Minimal frontend code required.
   - **Option B — Dash by Plotly (Python-only):** More customizable/production-like UI, steeper learning curve, still Python-based.
   - **Option C — Flask/FastAPI backend + separate JS frontend:** Most flexible, but requires real frontend (HTML/CSS/JS) skills and more setup time — higher risk for a 9-day remaining timeline.
   - **Default recommendation given the founder's profile:** **Streamlit**, because it converts existing pandas/SQL/DA skills directly into a working app with the least new-technology risk, and deploys free with almost no DevOps work. Confirm this choice explicitly at the start of Day 2 before proceeding — don't assume it.
2. **Define the architecture** (data flow, not code):
   ```
   Raw Dataset (CSV) 
        → Data Cleaning Script (Python/pandas) 
        → Cleaned Dataset (CSV/Parquet, stored in repo or loaded at runtime) 
        → App Layer (loads cleaned data, applies filters, computes aggregations) 
        → Visualization Layer (charts, map, insights text) 
        → Deployed Web App
   ```
3. **Define the data schema** — the target cleaned-data structure the app will consume. Minimum columns:
   - `accident_id`, `date`, `time`, `hour`, `day_of_week`, `latitude`, `longitude`, `region/city`, `severity`, `weather_condition`, `road_type`, `num_casualties` (if available).
4. **Sketch the UI layout** (can be a simple text/box wireframe, no design tool required):
   - Left/top: filter panel (dropdowns/sliders for region, date range, severity, weather).
   - Center: hotspot map.
   - Below/side: trend charts (time-of-day bar chart, day-of-week chart) + severity breakdown chart.
   - Top or side panel: auto-generated insights text block (2–4 bullet insights).
5. **Confirm folder/repo structure** (see below) so Day 3 setup has no ambiguity.

### 📂 Files and folders to create or modify
```
road-accident-explorer/
├── data/
│   ├── raw/              # original downloaded dataset
│   └── processed/        # cleaned dataset used by the app
├── src/
│   ├── data_cleaning.py  # ETL script
│   ├── insights.py       # logic that generates plain-language insights
│   └── app.py            # main Streamlit app
├── notebooks/             # exploratory analysis (optional, not shipped)
├── requirements.txt
├── README.md
└── .gitignore
```
(No files are created with content yet today — this is the agreed structure for Day 3 setup.)

### 🔗 APIs, libraries, services, or tools to integrate
- Decision only today — no integration yet. Candidates to research: `streamlit`, `pandas`, `plotly`, `folium` or `pydeck` (for maps), `streamlit-folium` (if using folium).

### 🧪 Testing tasks
- None (design day). Optional: run a 10-minute "hello world" Streamlit app locally to sanity-check the tool works on your machine before committing to it.

### 🐞 Common issues and debugging tips
- Don't over-design the wireframe — a rough text sketch is enough; the goal is agreement on layout, not pixel-perfect design.
- If undecided between Streamlit and Dash, default to Streamlit — it has the smallest gap between "I know pandas" and "I have a working app."

### ✅ End-of-day checklist
- [ ] Tech stack explicitly chosen and written down
- [ ] Architecture diagram (data flow) agreed
- [ ] Data schema (target columns) defined
- [ ] UI wireframe sketched (even if just text boxes)
- [ ] Folder structure agreed

### 📸 Expected project state and screenshots to capture
- Screenshot/text of the finalized tech stack decision and reasoning
- Screenshot of the wireframe sketch (hand-drawn, whiteboard tool, or text diagram)

### ➡️ Handoff notes for Day 3
Tech stack: [fill in after decision]. Architecture and schema: as defined above unless changed. Day 3 will set up the actual development environment and acquire the real dataset — bring the chosen stack name and confirmed schema into that conversation.

---

# Day 3 — Setup (Environment + Dataset Acquisition)

### 🎯 Objective
Get a working local development environment, a GitHub repository, and a real, validated road accident dataset in hand — cleaned enough to know it satisfies the Day 2 schema.

### 📖 What I'll learn
- How to structure a data science / app project repo professionally
- How to evaluate a public dataset for fitness-of-use before committing to it
- How to set up a Python virtual environment and dependency management

### 🛠 Features to build
No app features yet — this is infrastructure day.

### 📝 Step-by-step implementation plan
1. Create the GitHub repository (`road-accident-explorer` or similar) with the Day 2 folder structure.
2. Set up a Python virtual environment locally (`venv`) and install initial dependencies: `pandas`, `streamlit`, `plotly`, mapping library chosen Day 2.
3. Create `requirements.txt` listing exact packages.
4. Source the dataset: search for a public road accident dataset (e.g., Kaggle "US Accidents" dataset, or a national/regional government open-data road accident dataset) that contains the Day 2 required fields (date/time, location, severity, weather, road type).
5. Download the raw dataset into `data/raw/`.
6. Do a first-pass exploration (in a notebook or a quick script): check row count, column names, missing values, and confirm it can realistically support the schema from Day 2. If the ideal dataset is too large for free hosting, sample it down to a manageable size (e.g., one state/city/year) — note this decision for the README.
7. Write a one-paragraph "Data Source" note (dataset name, link, license, why chosen) for later use in the README and PRD appendix.

### 📂 Files and folders to create or modify
- `data/raw/<dataset_file>.csv` — downloaded dataset
- `requirements.txt` — initial dependencies
- `README.md` — project title, one-line description, data source note (placeholder for more detail later)
- `.gitignore` — exclude virtual environment folders, large data files if needed, `__pycache__`
- `notebooks/01_data_exploration.ipynb` (or `.py` script) — first-pass exploration

### 🔗 APIs, libraries, services, or tools to integrate
- GitHub (repo hosting)
- Kaggle or chosen open-data source (manual download — no paid API needed)
- `pandas` for initial exploration

### 🧪 Testing tasks
- Confirm the dataset loads without errors (`pd.read_csv` succeeds)
- Confirm required fields exist or can be derived (e.g., derive `hour`/`day_of_week` from a timestamp column)
- Confirm dataset size is reasonable for free-tier deployment (ideally under a few hundred MB; sample down if not)

### 🐞 Common issues and debugging tips
- Large datasets (millions of rows) can break free-tier memory limits — sample or filter to one region/year rather than fighting the full dataset.
- Watch for inconsistent date/time formats — note them now so Day 4's cleaning script handles them explicitly.
- If coordinates are missing but city/region names exist, note this now — hotspot map may need to aggregate at city/region level instead of exact points (a valid fallback, not a blocker).

### ✅ End-of-day checklist
- [ ] GitHub repo created with agreed folder structure
- [ ] Virtual environment set up, `requirements.txt` committed
- [ ] Dataset downloaded and confirmed loadable
- [ ] Dataset fields checked against Day 2 schema; gaps/fallbacks noted
- [ ] Data source note written for README

### 📸 Expected project state and screenshots to capture
- Screenshot of repo structure (GitHub or local file tree)
- Screenshot of dataset loaded in a notebook/terminal showing shape and columns

### ➡️ Handoff notes for Day 4
Dataset location: `data/raw/<filename>`. Known data issues to handle: [fill in from exploration]. Confirmed/adjusted schema: [fill in if it changed from Day 2]. Day 4 begins the real ETL/cleaning script.

---

# Day 4 — Implementation: Data Cleaning & ETL Pipeline

### 🎯 Objective
Turn the messy raw dataset into a clean, analysis-ready dataset matching the Day 2 schema, saved to `data/processed/`.

### 📖 What I'll learn
- Practical pandas data-cleaning patterns (missing values, type conversion, category standardization)
- How to engineer derived columns (hour, day-of-week) needed for later visualizations
- How to structure a repeatable, script-based (not notebook-only) cleaning pipeline

### 🛠 Features to build
- `src/data_cleaning.py` — a script that reads raw data and outputs cleaned data

### 📝 Step-by-step implementation plan
1. Load raw dataset in `data_cleaning.py`.
2. Standardize column names (lowercase, snake_case).
3. Parse date/time fields into proper datetime objects; derive `hour` and `day_of_week` columns.
4. Handle missing values: drop rows missing critical fields (location, date, severity); fill or flag less-critical missing fields (e.g., weather = "Unknown" if missing).
5. Standardize categorical fields (e.g., unify inconsistent weather/severity labels — check for typos/case differences).
6. Filter out invalid rows (e.g., impossible coordinates, negative casualty counts).
7. If applicable, aggregate/sample down to the manageable size decided on Day 3.
8. Save cleaned dataset to `data/processed/cleaned_accidents.csv` (or `.parquet` for smaller file size).
9. Print/log a short data quality summary (row count before/after, % missing per column) so you have a record of what was cleaned.

### 📂 Files and folders to create or modify
- `src/data_cleaning.py` — new file, the ETL script
- `data/processed/cleaned_accidents.csv` — output (generated, not hand-written)

### 🔗 APIs, libraries, services, or tools to integrate
- `pandas` (core cleaning)
- `numpy` (if needed for numeric handling)

### 🧪 Testing tasks
- Run the script end-to-end and confirm it completes without errors
- Spot-check the cleaned output: no nulls in critical columns, `hour` values range 0–23, `day_of_week` values are valid
- Confirm row count is reasonable (not accidentally dropping most of the data)

### 🐞 Common issues and debugging tips
- If parsing dates throws errors, print the unique malformed values first rather than guessing the format.
- If cleaning drops an unexpectedly large % of rows, check whether the "drop critical missing fields" rule is too strict — consider imputing instead of dropping for borderline cases.
- Keep the raw file untouched — always read from `data/raw/`, write to `data/processed/`, so cleaning can be safely re-run.

### ✅ End-of-day checklist
- [ ] `data_cleaning.py` runs successfully end-to-end
- [ ] `cleaned_accidents.csv` exists in `data/processed/` and matches the Day 2 schema
- [ ] Data quality summary reviewed (row counts, missing value %)
- [ ] No critical columns contain unexpected nulls

### 📸 Expected project state and screenshots to capture
- Screenshot of terminal output showing the cleaning script running successfully with summary stats
- Screenshot of the first few rows of the cleaned dataset (`df.head()`)

### ➡️ Handoff notes for Day 5
Cleaned dataset ready at `data/processed/cleaned_accidents.csv` with columns: [list final confirmed columns]. Day 5 builds the core analytics/aggregation logic and the insights engine on top of this file — no further cleaning should be needed unless a bug is found.

---

# Day 5 — Implementation: Core Analytics Logic & Insights Engine

### 🎯 Objective
Build the reusable Python logic that takes the cleaned dataset (plus active filters) and produces the aggregated numbers, chart-ready data, and plain-language insight sentences — independent of the UI, so Day 6–7 can plug straight into it.

### 📖 What I'll learn
- How to separate "logic" from "UI" (a core software engineering practice) so the app stays maintainable
- How to write functions that generate human-readable insight text from statistical summaries
- How to design filter logic that works generically across multiple dimensions (time, location, severity, weather)

### 🛠 Features to build
- A filtering function that takes the cleaned dataframe + selected filter values and returns the filtered subset
- Aggregation functions: accidents by hour, by day of week, by severity, by weather, by region
- `src/insights.py`: a function that takes the filtered/aggregated data and returns 3–5 plain-language insight strings

### 📝 Step-by-step implementation plan
1. In a new module (e.g., `src/analytics.py`), write `filter_data(df, filters: dict) -> pd.DataFrame` that applies all selected filters (region, date range, severity, weather) to the cleaned dataframe.
2. Write aggregation helper functions, e.g.:
   - `accidents_by_hour(df)`
   - `accidents_by_day_of_week(df)`
   - `accidents_by_severity(df)`
   - `accidents_by_weather(df)`
   - `hotspot_points(df)` — returns lat/lon (+ optional weight) for mapping
3. In `src/insights.py`, write `generate_insights(df) -> list[str]` that inspects the aggregated results and produces sentences such as:
   - "X% of accidents in this selection occurred between [peak hour range]."
   - "[Day of week] has the highest number of accidents in this selection."
   - "[Weather condition] is associated with the highest proportion of severe accidents."
   - Handle the empty-data case gracefully (e.g., "No accidents match the current filters.").
4. Test all functions directly (no UI yet) using sample filter combinations in a script or notebook, printing outputs to confirm correctness before wiring to the UI.

### 📂 Files and folders to create or modify
- `src/analytics.py` — new file: filtering + aggregation logic
- `src/insights.py` — new file: insight-generation logic
- (optional) `notebooks/02_logic_testing.ipynb` — scratch space to verify functions before UI integration

### 🔗 APIs, libraries, services, or tools to integrate
- `pandas` for all aggregation logic
- No new external services

### 🧪 Testing tasks
- Test `filter_data()` with: no filters, one filter, all filters combined, and a filter combination that returns zero rows
- Manually verify at least 2 insight sentences against a manual pandas `groupby` to confirm the numbers are correct
- Confirm insight text reads naturally (not robotic/broken grammar) for at least 3 different filter scenarios

### 🐞 Common issues and debugging tips
- If percentages in insights look wrong, double check you're dividing by the *filtered* count, not the full dataset count.
- Keep insight logic in plain functions (not tied to Streamlit) — this makes it independently testable and reusable later.
- Handle ties gracefully (e.g., two days with equal accident counts) — pick the first alphabetically or mention both, but don't let it crash.

### ✅ End-of-day checklist
- [ ] `filter_data()` works correctly for all filter combinations
- [ ] All aggregation functions return correct, verified numbers
- [ ] `generate_insights()` returns 3–5 correct, readable sentences
- [ ] Empty-filter-result case handled without crashing

### 📸 Expected project state and screenshots to capture
- Screenshot of terminal/notebook output showing filtered aggregation results
- Screenshot of a sample list of generated insight sentences

### ➡️ Handoff notes for Day 6
Core logic is ready in `src/analytics.py` and `src/insights.py`. Function signatures: `filter_data(df, filters)`, `accidents_by_hour(df)`, `accidents_by_day_of_week(df)`, `accidents_by_severity(df)`, `accidents_by_weather(df)`, `hotspot_points(df)`, `generate_insights(df)`. Day 6 builds the Streamlit UI (filters + charts) calling these functions directly — no new analytics logic should be needed, only UI wiring.

---

# Day 6 — Implementation: Dashboard UI — Filters & Charts

### 🎯 Objective
Build the main Streamlit app shell: the filter panel and the core trend/severity charts, wired to the Day 5 logic, running locally.

### 📖 What I'll learn
- How to build a multi-widget Streamlit app (selectboxes, sliders, date pickers)
- How to use Plotly for interactive charts inside Streamlit
- How to structure reactive UI code so charts update automatically when filters change

### 🛠 Features to build
- Filter panel: region/location, date range, severity, weather condition selectors
- Trend chart: accidents by hour of day
- Trend chart: accidents by day of week
- Severity breakdown chart

### 📝 Step-by-step implementation plan
1. In `src/app.py`, set up the Streamlit page config (title, layout=wide) and load the cleaned dataset once (cached with `@st.cache_data` for performance).
2. Build the filter panel in the sidebar using `st.sidebar`: region dropdown (`st.selectbox`/`st.multiselect`), date range (`st.date_input`), severity (`st.multiselect`), weather (`st.multiselect`).
3. Call `filter_data()` from `src/analytics.py` with the selected filter values to get the filtered dataframe — this should re-run automatically on any widget change (Streamlit's default behavior).
4. Add an "accidents by hour" bar chart using Plotly (`plotly.express.bar`), fed by `accidents_by_hour(filtered_df)`.
5. Add a "accidents by day of week" bar chart, same pattern.
6. Add a severity breakdown chart (pie or bar) using `accidents_by_severity(filtered_df)`.
7. Add a simple metric row at the top (e.g., `st.metric`) showing total accidents in the current filtered selection.
8. Run locally (`streamlit run src/app.py`) and manually test that changing each filter updates all charts.

### 📂 Files and folders to create or modify
- `src/app.py` — main app file, built out significantly today
- Imports from `src/analytics.py` and (next day) `src/insights.py`

### 🔗 APIs, libraries, services, or tools to integrate
- `streamlit` (UI framework)
- `plotly.express` (charts)

### 🧪 Testing tasks
- Test each filter individually and in combination — confirm charts update correctly
- Test the zero-results case (a filter combination with no matching data) — confirm no crash, a friendly message shows instead
- Test app performance — filter changes should feel responsive, not laggy

### 🐞 Common issues and debugging tips
- If the app feels slow, confirm `@st.cache_data` is applied to the data-loading function so the CSV isn't re-read on every interaction.
- If charts don't update, check that you're re-computing aggregations from the *filtered* dataframe inside the main app flow, not from a cached/stale copy.
- Streamlit reruns the whole script top-to-bottom on every widget interaction — this is expected behavior, not a bug.

### ✅ End-of-day checklist
- [ ] Filter panel built and functional (region, date, severity, weather)
- [ ] Hour-of-day and day-of-week trend charts working and reactive
- [ ] Severity breakdown chart working and reactive
- [ ] Total-accidents metric displayed and updates with filters
- [ ] App runs locally without errors

### 📸 Expected project state and screenshots to capture
- Screenshot of the app running locally showing filter panel + charts with default (unfiltered) data
- Screenshot showing the same view after applying 2–3 filters, demonstrating charts changed

### ➡️ Handoff notes for Day 7
`src/app.py` has a working filter panel and trend/severity charts, all reactive. Filtered dataframe variable name: `filtered_df` (or your chosen name — note it here). Day 7 adds the hotspot map and the insights panel on top of this same file, plus overall UI polish — no new filters should be needed unless a gap was found today.

---

# Day 7 — Implementation: Hotspot Map, Insights Panel & UI Polish

### 🎯 Objective
Complete the core v1.0 feature set by adding the accident hotspot map and the auto-generated insights panel, then polish the overall UI so it feels like a finished product, not a prototype.

### 📖 What I'll learn
- How to build an interactive map visualization in a Streamlit app
- How to present computed insights in a clean, readable UI panel
- How to apply basic UI polish (layout, spacing, titles) that meaningfully improves perceived quality

### 🛠 Features to build
- Accident hotspot map (using the mapping library chosen Day 2 — e.g., `pydeck` or `folium` via `streamlit-folium`)
- Insights panel displaying `generate_insights()` output
- Overall layout polish: page title, section headers, consistent spacing, sensible chart ordering

### 📝 Step-by-step implementation plan
1. Import `hotspot_points()` from `src/analytics.py`, feed it the filtered dataframe.
2. Render the map:
   - If using `pydeck`: build a `HexagonLayer` or `ScatterplotLayer` on `st.pydeck_chart`.
   - If using `folium`: build a `folium.Map` with a `HeatMap` or marker cluster layer, render via `streamlit-folium`'s `st_folium`.
   - If precise coordinates are sparse/unreliable (noted on Day 3), aggregate to region/city-level circles sized by accident count instead of exact points — this is a valid, intentional fallback, not a shortcut to be ashamed of.
3. Import `generate_insights()` from `src/insights.py`, call it with the filtered dataframe, and render the returned sentences as a clean bulleted list or styled callout box (e.g., `st.info` per insight, or a custom panel).
4. Reorganize the page layout: e.g., insights panel + key metric at top, map next, trend/severity charts below, using `st.columns` where helpful for a two-column layout.
5. Add a clear page title, short one-line app description under the title, and section headers above each visualization group.
6. Do a full pass for visual polish: consistent chart heights, no orphaned/empty sections, meaningful chart titles and axis labels (not raw column names).
7. Run the full app locally end-to-end, click through every filter combination once more.

### 📂 Files and folders to create or modify
- `src/app.py` — extended with map + insights panel + layout polish
- `src/analytics.py` — only if `hotspot_points()` needs adjustment based on real map testing

### 🔗 APIs, libraries, services, or tools to integrate
- `pydeck` or `folium` + `streamlit-folium` (whichever was chosen Day 2)

### 🧪 Testing tasks
- Confirm the map renders and updates correctly when filters change
- Confirm insights text updates correctly and makes sense for at least 4 different filter scenarios
- Full click-through test: apply every filter type at least once, confirm nothing breaks or looks unfinished
- Check readability: chart titles, axis labels, and insight text are in plain English, not raw column/variable names

### 🐞 Common issues and debugging tips
- Map libraries can be the trickiest part of a Streamlit app — if the chosen library causes persistent issues, it's acceptable to fall back to a simpler visualization (e.g., a bar chart of "accidents by region" instead of a geographic map) rather than losing a day to a single widget. Note this fallback decision here if used.
- If insights read awkwardly, favor simpler, more direct sentence templates over "clever" ones — clarity beats cleverness for this audience.
- Keep an eye on total page load time — too many heavy visualizations can slow things down; simplify if needed.

### ✅ End-of-day checklist
- [ ] Hotspot map implemented and reactive to filters (or documented fallback used)
- [ ] Insights panel implemented and reactive to filters
- [ ] Full page layout polished (titles, headers, spacing, chart labels)
- [ ] Full manual click-through test passed with no crashes or broken states

### 📸 Expected project state and screenshots to capture
- Screenshot of the complete app (all sections visible) with default filters
- Screenshot of the complete app after applying multiple filters, showing map + insights + charts all updated together

### ➡️ Handoff notes for Day 8
All core v1.0 features are implemented and working locally: filters, hotspot map, trend charts, severity breakdown, insights panel. The app is feature-complete but not yet formally tested or deployed. Day 8 focuses purely on testing and bug-fixing — no new features should be added unless a testing gap reveals a genuine functional hole (not a "nice to have").

---

# Day 8 — Testing

### 🎯 Objective
Systematically test the complete app to find and fix bugs, edge cases, and rough edges before deployment — this is quality assurance day, not feature-building day.

### 📖 What I'll learn
- How to write and run a manual test plan for a data application
- How to think in edge cases (empty states, extreme filters, malformed data)
- How to prioritize bug fixes under a tight deadline (critical vs. cosmetic)

### 🛠 Features to build
None new — bug fixes only, scoped strictly to features already built.

### 📝 Step-by-step implementation plan
1. Write a simple manual test checklist covering:
   - Each filter individually (region, date range, severity, weather)
   - Combinations of 2–3 filters together
   - Edge cases: filter combination returning zero results, selecting all options, selecting a very narrow date range
   - Full reset (clearing all filters back to default)
2. Execute the checklist methodically, logging any bug found (what you did, what happened, what should have happened) in a simple running list (can be a section in the README or a `TESTING.md` file).
3. Fix bugs in priority order:
   - **Critical:** app crashes, blank/broken charts, incorrect numbers
   - **Moderate:** confusing UI labels, slow performance, awkward insight phrasing
   - **Cosmetic:** minor spacing/alignment issues
4. Re-test after each fix to confirm resolution and check nothing else broke.
5. Do a "fresh eyes" pass: if possible, have someone else (friend/classmate) click through the app with zero explanation and note where they get confused — this simulates your real end user.
6. Check performance: time how long the app takes to respond to a filter change; if it's noticeably slow, look at caching (`@st.cache_data`) or dataset size as the likely fix.

### 📂 Files and folders to create or modify
- `TESTING.md` — new file: test checklist + bug log (useful evidence for the capstone submission too)
- Bug fixes will touch `src/app.py`, `src/analytics.py`, or `src/insights.py` as needed — no new files expected

### 🔗 APIs, libraries, services, or tools to integrate
None new.

### 🧪 Testing tasks
This entire day *is* the testing task — see step-by-step plan above.

### 🐞 Common issues and debugging tips
- If a specific filter combination consistently causes a crash, add a defensive check (e.g., "if filtered dataframe is empty, show a friendly message and skip chart rendering" instead of letting Plotly/pydeck fail on empty data).
- If performance is the issue, first check whether data loading is cached — that's the most common cause of "everything feels slow."
- Resist the urge to add new features while testing — write them down as Future Scope ideas instead, and stay focused on making what exists solid.

### ✅ End-of-day checklist
- [ ] Manual test checklist executed fully
- [ ] All critical bugs fixed and re-tested
- [ ] Moderate bugs fixed where time allows; remaining ones documented
- [ ] `TESTING.md` committed with the checklist and bug log
- [ ] App performance feels acceptable (filter changes respond within a few seconds)

### 📸 Expected project state and screenshots to capture
- Screenshot of the `TESTING.md` checklist/bug log
- Screenshot of the app functioning correctly on a previously-buggy scenario (before/after if possible)

### ➡️ Handoff notes for Day 9
App is functionally tested and stable locally. Known non-critical issues (if any) are documented in `TESTING.md`. Day 9 is deployment day — no further feature or bug work should be needed beyond what deployment itself surfaces (e.g., environment-specific issues).

---

# Day 9 — Deployment

### 🎯 Objective
Deploy the app to a free, public hosting platform so it's accessible via a shareable URL, and confirm it works correctly in the live environment (not just locally).

### 📖 What I'll learn
- How to deploy a Streamlit app to Streamlit Community Cloud (or equivalent free platform)
- How to manage environment/dependency differences between local and deployed environments
- How to do post-deployment QA

### 🛠 Features to build
None new — deployment configuration only.

### 📝 Step-by-step implementation plan
1. Confirm `requirements.txt` is complete and accurate (every import used in `src/` is listed with a version, generated via `pip freeze` or manually verified).
2. Confirm the cleaned dataset is either committed to the repo (`data/processed/`) or otherwise accessible at runtime — free-tier deployment needs the data available without manual upload steps.
3. Push the final code to GitHub (main branch).
4. Deploy via Streamlit Community Cloud (free): connect the GitHub repo, point it to `src/app.py` as the entry file, deploy.
5. Wait for build to complete; check build logs for missing dependency or path errors.
6. Once live, open the public URL and re-run a shortened version of the Day 8 test checklist directly on the deployed version (filters, map, insights, edge cases) — deployed environments sometimes behave differently from local (file paths, package versions).
7. Fix any deployment-specific issues (commonly: file path issues, missing dependency, memory limits on large datasets) and redeploy.
8. Save the final public URL — this is a core Day 10 deliverable.

### 📂 Files and folders to create or modify
- `requirements.txt` — finalized, verified
- Possibly a `.streamlit/config.toml` if any display settings need adjusting (optional)
- `README.md` — add the live app link once deployed

### 🔗 APIs, libraries, services, or tools to integrate
- Streamlit Community Cloud (free hosting) — or the equivalent free platform matching the Day 2 stack decision, if different
- GitHub (source for deployment)

### 🧪 Testing tasks
- Full click-through of the live deployed app (not local) covering all filters, map, insights, edge cases
- Confirm load time is acceptable on the live version
- Test on both desktop and mobile browser widths, since users may access via phone

### 🐞 Common issues and debugging tips
- **"Module not found" on deploy:** almost always a missing entry in `requirements.txt` — check the build log for the exact package name.
- **Data file not found:** confirm the path used in code is relative to the repo root (not an absolute local path) and that the data file is actually committed to GitHub (check `.gitignore` isn't excluding it).
- **App works locally but slow/crashes when deployed:** likely a memory limit — reduce dataset size or add more aggressive caching.
- **Map doesn't render on deploy:** some mapping libraries need extra system dependencies; check the platform's docs for known Streamlit + mapping-library deployment notes, or fall back to the simpler visualization noted as a Day 7 contingency.

### ✅ End-of-day checklist
- [ ] `requirements.txt` finalized and verified
- [ ] Code pushed to GitHub main branch
- [ ] App successfully deployed and publicly accessible
- [ ] Full test checklist re-run on the live deployed app
- [ ] Public URL saved and added to README

### 📸 Expected project state and screenshots to capture
- Screenshot of the deployment platform showing successful build/deploy status
- Screenshot of the live app open in a browser at its public URL
- Screenshot of the live app working correctly on a mobile-width browser view

### ➡️ Handoff notes for Day 10
App is live at: [public URL]. All core features confirmed working in production. Day 10 is final polish, documentation, and submission — not further feature work, unless the live testing above revealed something critical still broken.

---

# Day 10 — Maintenance, Polish & Submission

### 🎯 Objective
Finalize documentation, polish any remaining rough edges, prepare the demo/presentation materials, and submit the completed capstone — arriving at a genuinely "done" v1.0.

### 📖 What I'll learn
- How to write documentation that lets someone else understand and run your project
- How to present a finished technical project clearly and confidently
- What "maintenance" means in a real SDLC — closing the loop, not just shipping and walking away

### 🛠 Features to build
None — this is polish, documentation, and packaging day. Any last-minute fix should be small (typos, minor UI tweaks) — anything bigger is a sign scope wasn't controlled earlier, and should be logged as Future Scope rather than rushed in today.

### 📝 Step-by-step implementation plan
1. Finalize `README.md` with:
   - Project name and one-paragraph description (reuse/adapt from the PRD)
   - Live app link
   - Screenshot of the app
   - Feature list (from PRD "in scope")
   - Tech stack used
   - How to run locally (setup instructions)
   - Data source and license note
   - Future scope ideas (predictive ML, live data, accounts, etc.)
2. Do one final full click-through of the live app as if you were a brand-new user — this is your last chance to catch anything confusing before presenting it.
3. Prepare your demo flow: a simple 3–5 step walkthrough script (e.g., "open the app → show default view → apply a time filter → point out the auto-generated insight → show the hotspot map update") to use when presenting.
4. Re-open the Pitch Deck (generated Day 1) and do a final check that it still accurately reflects the finished product — update the "Key Features" or "Technical Approach" slide text if anything changed meaningfully during the build.
5. Do the GitHub submission steps for the challenge (Day51 folder, day51.md file, screenshots, PRD, blueprint, pitch deck, key learnings) as instructed by the challenge process.
6. Write a short "Key Learnings" reflection (a few sentences to a paragraph) covering: what was technically hardest, what you'd do differently, and what you're proudest of — this is both good practice and often explicitly required for submission.

### 📂 Files and folders to create or modify
- `README.md` — finalized
- `Day51/day51.md` (or equivalent per challenge instructions) — submission summary + key learnings
- No changes expected to `src/` unless a genuine last-minute bug is found

### 🔗 APIs, libraries, services, or tools to integrate
None new.

### 🧪 Testing tasks
- Final full click-through on the live deployed app
- Confirm all links in the README (live app, dataset source) work correctly
- Proofread README and submission files for typos/clarity

### 🐞 Common issues and debugging tips
- Resist "just one more feature" — Day 10 is about finishing cleanly, not adding scope. If something feels missing, it belongs in the README's "Future Scope" section, not in a rushed last-minute commit.
- If the app has been idle on a free hosting tier, do a final check that it "wakes up" correctly before any live demo — some free tiers sleep after inactivity.

### ✅ End-of-day checklist
- [ ] README fully finalized with live link, screenshots, setup instructions, data source, future scope
- [ ] Final full click-through of live app completed with no issues
- [ ] Pitch deck reviewed and still accurate to the finished product
- [ ] Key Learnings reflection written
- [ ] All challenge submission steps completed (folder, file, screenshots, deliverables, commit, push)
- [ ] GitHub commit URL ready to submit

### 📸 Expected project state and screenshots to capture
- Screenshot of the finalized README on GitHub
- Screenshot of the live app (for the submission)
- Screenshot of the final GitHub commit/repo state

### ➡️ Handoff notes
This is the final day — no handoff needed. If continuing to iterate post-capstone, the natural next steps are the "Future Scope" items already defined in the PRD and Pitch Deck: predictive ML for high-risk zone forecasting, live data feeds, and multi-dataset support.

---

## 🧭 Parking Lot (ideas surfaced mid-build that are explicitly NOT for v1.0)

Use this section across all days to capture good ideas without breaking scope discipline. Nothing here should be built before Day 10 unless the PRD is explicitly re-approved.

- (Add items here as they come up during the build)
