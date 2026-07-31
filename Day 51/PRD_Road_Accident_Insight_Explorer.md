# Product Requirements Document (PRD)
## Road Accident Insight Explorer

**Prepared for:** AB Talks 60-Day Claude AI Challenge — 10-Day Capstone
**Document owner:** [Your Name]
**Date:** Day 1 of 10 (Requirements Phase)
**Status:** ✅ Approved — Foundation for Days 2–10

---

## 1. Project Overview

Road Accident Insight Explorer is an interactive, deployed web application that turns raw, messy road accident data into visual, self-serve insights for anyone — regardless of technical skill. Instead of requiring SQL queries, Power BI licenses, or Excel wrangling, a user opens a link, applies simple filters, and immediately sees where, when, and why accidents are concentrated.

This project is a direct evolution of the founder's self-placed capstone (road accident datasets + data warehousing), addressing the specific pain point identified during that work: **the data was hard to explore for anyone who wasn't the analyst who built it.**

---

## 2. Problem Statement

Road accident datasets (government open data, city traffic records, insurance datasets) are:

- **Large and messy** — inconsistent categorical codes, missing values, mixed formats for time/location/severity.
- **Locked behind technical tools** — genuinely useful patterns exist in the data, but extracting them today requires SQL, Power BI, or Excel skills most stakeholders (police, planners, students, citizens) don't have.
- **Presented as static reports**, not living tools — a PDF or a fixed dashboard answers only the questions the analyst thought to ask, not the ones a new viewer actually has.

As a result, valuable safety insights (high-risk intersections, dangerous time windows, weather-related risk factors) stay locked inside spreadsheets instead of informing real decisions or public awareness.

---

## 3. Target Users / Personas

| Persona | Description | Need |
|---|---|---|
| **City/Traffic Planner** | Works for local government or municipal transport body | Wants to identify high-risk zones and times to prioritize interventions (signage, patrols, road redesign) |
| **Police / Law Enforcement Analyst** | Reviews accident trends for enforcement planning | Wants quick answers: "when and where should we increase patrols?" |
| **Student / Researcher** | Studying road safety, public policy, or data science | Wants to explore the dataset for coursework or research without writing SQL |
| **Curious Citizen** | General public, journalists | Wants an accessible way to understand road safety in their area |

**Primary persona for v1.0:** City Planner / Analyst-adjacent non-technical user — because designing for the least technical serious user (not a fellow analyst) forces the interface to be genuinely self-serve.

---

## 4. Goals & Success Metrics

### Product Goals
1. Make road accident data explorable without any technical skill.
2. Surface genuine, non-obvious patterns automatically (not just raw charts).
3. Ship a live, publicly accessible v1.0 by Day 10.

### Success Metrics (Day 10 Definition of Done)
- ✅ App is deployed and accessible via a public URL.
- ✅ A first-time user with zero technical background can apply at least 3 different filters without confusion.
- ✅ The app surfaces at least 3 auto-generated plain-language insights (e.g., "62% of accidents in this filter occurred between 5–8 PM").
- ✅ The hotspot map and trend charts update correctly when filters change.
- ✅ No critical bugs (crashes, broken filters, blank charts) in the deployed version.
- ✅ App loads and responds within a few seconds on a typical connection.

---

## 5. Scope

### 5.1 In Scope — v1.0 Features

| # | Feature | Description |
|---|---|---|
| 1 | **Dataset ingestion & cleaning** | One public road accident dataset, cleaned and structured for analysis |
| 2 | **Filter panel** | Filter by location/region, date/time range, severity, weather condition, road type |
| 3 | **Accident hotspot map** | Geographic visualization showing concentration of accidents |
| 4 | **Trend charts** | Accidents over time (by hour of day, day of week, month) |
| 5 | **Severity breakdown** | Visual split of accidents by severity level |
| 6 | **Auto-generated insights panel** | Plain-language summary statements that update based on active filters |
| 7 | **Public deployment** | Live, shareable link, no login required |

### 5.2 Explicitly Out of Scope for v1.0

| Excluded | Reason |
|---|---|
| Real-time/live accident data feeds | Requires live data infrastructure; not needed to prove the core value |
| Predictive ML (forecasting future accidents/hotspots) | High complexity/risk for a 10-day build; strong candidate for Future Scope |
| User accounts / login / saved preferences | Adds auth complexity with no core-value payoff for v1.0 |
| Native mobile app | Web app is accessible on mobile browsers; native app is unnecessary scope |
| Multi-language support | Not essential to prove the concept; can be added later |
| Multi-dataset merging (e.g., combining multiple countries/cities) | One well-cleaned dataset is enough to demonstrate the product |

**Scope discipline rule for the rest of this capstone:** if a new idea doesn't serve the Day 10 success metrics above, it goes into Future Scope — not into v1.0.

---

## 6. Functional Requirements

| ID | Requirement |
|---|---|
| FR-1 | System shall load and display a cleaned road accident dataset on app start |
| FR-2 | User shall be able to filter data by at least: location/region, time period, severity, weather |
| FR-3 | System shall render a map showing accident density/hotspots based on active filters |
| FR-4 | System shall render at least two trend charts (time-of-day and day-of-week, at minimum) |
| FR-5 | System shall render a severity distribution visualization |
| FR-6 | System shall auto-generate at least 3 plain-language insight statements that recompute when filters change |
| FR-7 | All visualizations shall update reactively when filters are changed, without requiring a page reload |
| FR-8 | The application shall be accessible via a public URL without requiring login |

---

## 7. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-1 | **Usability:** A non-technical first-time user must be able to use core filters without instructions |
| NFR-2 | **Performance:** Filter changes should reflect in visualizations within a few seconds |
| NFR-3 | **Reliability:** No crashes on empty filter results — show a graceful "no data" state instead |
| NFR-4 | **Cost:** Built and deployed entirely on free-tier tools and services |
| NFR-5 | **Maintainability:** Codebase organized so a fresh AI session on any future day can understand and extend it without re-reading the entire history |
| NFR-6 | **Accessibility:** Reasonable color contrast and readable font sizes on charts/text |

---

## 8. User Stories

- *As a city planner*, I want to filter accidents by time of day, so I can identify when road patrols are most needed.
- *As a police analyst*, I want to see a hotspot map, so I can identify which intersections need enforcement attention.
- *As a student researcher*, I want to filter by weather condition, so I can study the relationship between weather and accident severity.
- *As a curious citizen*, I want to read plain-language insights, so I can understand accident patterns in my area without analyzing raw data myself.
- *As any user*, I want the charts to update instantly when I change filters, so I can explore freely without waiting or reloading.

---

## 9. Data Requirements

- **Source:** A public, freely available road accident dataset (final dataset to be selected and confirmed on Day 3 — Setup phase). Candidate types: national/regional road accident open-data records (e.g., government road safety datasets, Kaggle road accident datasets).
- **Required fields (minimum):**
  - Date & time of accident
  - Location (coordinates or region/city name)
  - Severity level
  - Weather condition
  - Road type / junction type (if available)
- **Data volume:** Should be large enough to show meaningful patterns (thousands of records) but manageable within free-tier hosting/compute limits.
- **Data quality handling:** Missing values, inconsistent categories, and outliers will be cleaned during the Setup/Data phase (Days 3–4) before any visualization work begins.

---

## 10. Assumptions & Constraints

**Assumptions**
- A suitable public dataset with the required fields is available for free download.
- The founder has working knowledge of Python basics, SQL, Excel, and Power BI, but is not yet an experienced app developer — tooling choices (finalized Day 2) will account for this.
- 10 total days are available, with meaningful daily time investment.

**Constraints**
- No paid tools, services, APIs, or hosting may be used.
- The application must be deployable without requiring the end user to install anything.
- Development time is fixed at 9 remaining days (Days 2–10) after today.

---

## 11. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Dataset lacks clean geographic data for mapping | Hotspot map feature blocked | Validate dataset fields on Day 3 before committing; have a fallback (region-level aggregation instead of precise coordinates) |
| Scope creep (adding predictive ML, accounts, etc. mid-sprint) | Missed Day 10 deadline | This PRD's "Out of Scope" list is the single source of truth; any new idea goes to Future Scope |
| Free-tier hosting limits (compute/storage) | App slow or fails to deploy | Choose lightweight stack and dataset size during Day 2 design decision; test deployment early (Day 9, not Day 10) |
| Founder unfamiliar with chosen app framework | Slower implementation days | Day 2 will select a framework matching existing Python/DA skills to minimize learning curve |

---

## 12. Timeline — SDLC Mapping (10 Days)

| Day | SDLC Phase | Focus |
|---|---|---|
| 1 (Today) | Requirements | Discovery, scoping, PRD, blueprint, pitch deck |
| 2 | Design | Tech stack decision, architecture, data schema, UI wireframes |
| 3 | Setup | Environment setup, repo structure, dataset acquisition & validation |
| 4 | Implementation | Data cleaning & ETL pipeline |
| 5 | Implementation | Core analytics logic & insights engine |
| 6 | Implementation | Dashboard UI — filters & charts |
| 7 | Implementation | Hotspot map, insights panel, UI polish |
| 8 | Testing | Functional testing, edge cases, bug fixes |
| 9 | Deployment | Deploy live, deployment QA |
| 10 | Maintenance/Wrap-up | Final polish, documentation, demo prep, submission |

---

## 13. Appendix

- **Full technical implementation details** (including tech stack decision, file structure, and day-by-day build plan) are documented separately in the **Implementation Blueprint (Days 2–10)** — the single source of truth for execution.
- **This PRD is the source of truth for scope.** Any conflict between later technical decisions and this PRD should be resolved in favor of this document unless explicitly renegotiated and re-approved.
