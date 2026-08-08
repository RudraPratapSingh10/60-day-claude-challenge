# Challenge Retrospective — Road Accident Insight Explorer

**A 10-day capstone, built as the final practical milestone of the AB Talks 60-Day Claude AI Challenge.**

## The build, day by day

**Day 1 — Requirements.** Started from a real pain point (a prior data-warehousing capstone that was hard for anyone but the analyst who built it to explore) and turned it into a proper PRD: personas, functional/non-functional requirements, and — critically — an explicit "Out of Scope" list that got referenced and defended on almost every day that followed.

**Day 2 — Design.** Confirmed Streamlit as the stack (matching existing pandas/SQL/Power BI skills, zero learning-curve tax). Made one architecture call that paid off repeatedly: choosing Plotly's `density_mapbox` for the hotspot map instead of adding a second mapping library (folium/pydeck), specifically to reduce deployment risk. That decision was still saving effort six days later.

**Day 3 — Setup.** Hit the first real environment bug: Windows' Application Control policy silently blocked a brand-new pandas release (3.0.5) from importing. Fixed by pinning to the established `pandas==2.2.3` — a small decision that quietly mattered again on Day 9. Sourced the real dataset: 117,536 UK road accident records from the DfT, via Kaggle.

**Day 4 — ETL pipeline.** Built `data_cleaning.py`, decoding the UK government's STATS19 numeric category codes into readable labels. 99.98% row retention. Discovered mid-build that `Local_Authority_(District)` was a numeric code, not a name — filed for later rather than derailing the day.

**Day 5 — Analytics & insights engine.** Built the filtering and aggregation logic, fully decoupled from the UI so it could be tested independently. Caught a real bug during testing: filtering to "Fatal only" made the weather-severity insight say something technically true but meaningless ("100% severe in Clear weather") — fixed by detecting and skipping that insight when it can't add information.

**Day 6 — Dashboard UI.** Wired the tested logic into an actual filter panel and charts. Hit a genuinely obscure bug: invisible corrupted characters inside `.gitignore`, silently defeating exclusion rules that looked correct in every visual inspection — solved by rewriting the file directly through the terminal instead of an editor paste.

**Day 7 — Map, insights panel, UX polish.** Added the hotspot map (initially just scattered dots — fixed with a proper heat gradient and larger blend radius) and the insights panel. Did a full UX pass: custom theme, tooltips, loading/error states, an "About this data" trust section.

**Day 8 — Testing.** The most bug-dense day of the build, on purpose. Found and fixed three real, non-cosmetic bugs: a "Reset all filters" button that looked functional but silently did nothing (Streamlit widget state persists without an explicit reset), a fragile map color scale, and — the biggest one — confirmation that the Day 4 region field was showing meaningless numeric codes to end users. Fixed properly by sourcing the UK government's own official Local Authority District lookup table and re-running the cleaning pipeline.

**Day 9 — Deployment.** Shipped to Streamlit Community Cloud. Hit one last real bug on the way out the door: the cloud platform silently defaulted to Python 3.14, which had no pre-built package available for the `pandas==2.2.3` pinned back on Day 3 — the build hung indefinitely with no error message. `runtime.txt` (the documented fix) didn't work, a known current platform limitation. Solved by deleting the deployment and redeploying with Python 3.13 explicitly selected.

## Major technical decisions

- **Streamlit monolith, no REST API layer** — matched to the actual scope (no external clients, no mobile app).
- **Rule-based insights, not an LLM call** — deterministic, free, no API key risk, and arguably more trustworthy for a public-safety dataset than a generative summary would be.
- **Flat-file Parquet, no database** — the dataset is static and read-only; a database would have added operational complexity with zero benefit.
- **One shared mapping library (Plotly), not two** — a small Day 2 call that reduced Day 7 and Day 9 risk.

## Skills demonstrated

Requirements gathering and scope discipline · system architecture and documented trade-offs · ETL and data cleaning against a real, messy government dataset · building and independently testing business logic before UI work · UX design (theming, empty/loading/error states, accessibility basics) · systematic QA and bug triage · debugging genuinely obscure issues (invisible file corruption, silent widget state, platform-specific dependency resolution) · cloud deployment and production debugging · technical documentation as a first-class deliverable, not an afterthought.

## Lessons learned

The bugs that mattered most weren't the ones that crashed the app — they were the ones that looked fine. A button that runs without erroring but does nothing. A filter dropdown that renders without erroring but shows meaningless numbers. A map that draws without erroring but communicates nothing. Every one of those needed a human looking at the actual output, not just a green checkmark, to catch. That's the real argument for the deliberate structure of this build: separating logic from UI so each could be tested on its own terms, and treating Day 8 as a dedicated day rather than an afterthought.

## A note from your AI pair programmer

We started this on Day 1 with a PRD and an "Out of Scope" list, and — genuinely unusually for a project like this — that list held. Every day we caught something (a Windows security policy, an invisible corrupted character, a button that lied about resetting, a numeric code masquerading as a place name, a Python version nobody asked for), and every time, the instinct was the same: stop, understand it properly, fix the real cause, and keep moving. That's the whole craft, really. Nine days ago this was an empty repository. Today it's a live, public, tested application that a city planner, a researcher, or just a curious person could actually use. That's worth being proud of.
