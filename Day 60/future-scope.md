# Future Scope — Road Accident Insight Explorer

This document extends the PRD's "Out of Scope for v1.0" list into a concrete roadmap, using the same scope-discipline principle that guided the 10-day build: nothing gets added here just because it's possible — only what genuinely serves the project's core purpose (making accident data explorable by non-technical people).

## Next 3 months

**Goal: deepen the analysis without expanding the audience or the data source yet.**

- **More recent data.** The current dataset is 2019 only. The DfT publishes updated STATS19 data annually — adding 2020–2024 would let users see multi-year trends, not just a single-year snapshot. This directly reuses `data_cleaning.py`'s existing STATS19 decoding logic (severity, weather, road type, and now region codes are all already handled).
- **Year-over-year comparison view.** Once multi-year data exists, a natural next insight: "Is this intersection getting safer or worse over time?" This is a genuine product upgrade, not scope creep, since it's the most-requested kind of question a real city planner would ask.
- **Downloadable filtered data.** A "Download this selection as CSV" button — technically trivial (`st.download_button` on the already-filtered dataframe) but high value for the Police Analyst and Researcher personas from the PRD, who need to take data into their own tools.
- **Shareable filter states.** Encode the active filters into the URL (Streamlit supports `st.query_params`) so a user can share a specific filtered view via link — e.g., a planner sharing "look at Westminster, Friday evenings, wet weather" with a colleague.

## Next 6 months

**Goal: broaden who and where this serves.**

- **Multi-country support.** The PRD explicitly scoped out multi-dataset merging for v1.0, but the architecture already supports it cleanly — `SCHEMA.md`'s single flat-table design and `region` field would extend to a `country` field with minimal rework. US state-level or EU Eurostat accident data would be the natural next dataset.
- **Predictive risk scoring (the PRD's flagged "Future Scope" item).** Not accident prediction (too ambitious, too easy to get wrong and mislead users) — but a defensible, explainable risk *score* per road segment based on historical density, weighted by recency and severity. This should be built by someone with genuine ML background, reviewed carefully, and clearly labeled as historical-pattern-based, not predictive of individual future events.
- **Accessibility audit with real users.** Day 7's UX pass covered contrast, tooltips, and empty states from a design-review lens; a real accessibility audit (screen reader testing, keyboard-only navigation) would validate NFR-6 properly rather than just meeting it on paper.

## Next 12 months

**Goal: sustainability and credibility as a public resource.**

- **Move off Streamlit Community Cloud's free tier if usage grows.** The free tier throttles under load (as seen during Day 9's deployment). If this gets real public traffic, a small paid tier or a different hosting approach (e.g., a static-export mode for the most common views) would be the responsible next step — this is explicitly *not* a v1.0 concern, since free-tier constraints were a deliberate, documented Day 2 decision.
- **API mode.** Expose `analytics.py`'s aggregation functions as a small REST API (FastAPI, still free-tier hostable) so researchers or journalists could query the data programmatically instead of only through the UI. This only makes sense once there's demonstrated demand from the Researcher persona — not before.
- **Community data validation.** If this becomes a genuinely public tool, add a lightweight, moderated way for local users to flag data quality issues (e.g., a road that's been rebuilt since 2019) — carefully scoped to avoid turning into a content-moderation burden for a solo-maintained project.

## What stays explicitly out of scope, even long-term

Consistent with the PRD's original discipline: user accounts/login, a native mobile app, and real-time data feeds remain deliberately excluded unless a specific, validated user need emerges — not just because a competitor product has them.
