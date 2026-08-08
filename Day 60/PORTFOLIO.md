# Portfolio Materials — Road Accident Insight Explorer

## Project descriptions

**One-liner (for a portfolio site header or LinkedIn "Featured" section):**
> Road Accident Insight Explorer — an interactive Streamlit app turning 117K+ UK road accident records into a filterable hotspot map, trend charts, and auto-generated insights, deployed live and built end-to-end in a 9-day sprint.

**Short (2-3 sentences, for a portfolio card or resume project summary):**
> Built and deployed a full-stack data application that lets non-technical users explore 117,508 real UK road accident records through interactive filters, a hotspot density map, and auto-generated plain-language insights — no SQL or BI tools required. Designed the complete system from PRD through production deployment, including an ETL pipeline that decodes official UK government STATS19 category codes and a dedicated QA pass that caught and fixed three real, non-cosmetic bugs before launch.

**Long (for a case study page or detailed portfolio entry):**
> Road Accident Insight Explorer addresses a specific, real problem: government road-safety open data is genuinely valuable but locked behind tools (SQL, Power BI, Excel) that most stakeholders — city planners, police analysts, researchers, curious citizens — don't have access to or comfort with. I designed and built a complete Streamlit application that turns 117,508 real UK Department for Transport accident records into something anyone can explore with a click.
>
> The build followed a disciplined 9-day SDLC: requirements and scope definition, system architecture and schema design, environment setup, an ETL pipeline decoding official government coding schemes, an independently-tested analytics/insights engine, a full dashboard UI (filters, hotspot map, trend charts), a dedicated QA day that surfaced three real production bugs (including a "Reset" button that silently did nothing, and a region filter showing meaningless numeric codes instead of place names — traced back and fixed by sourcing the government's own official code lookup table), and a production deployment with a genuine cloud-environment bug fixed along the way (a Python version mismatch that hung the build with no error message).
>
> The result is a live, publicly deployed, zero-cost application (Streamlit Community Cloud, no database, no paid APIs) with full technical documentation covering architecture, schema, and testing.

## Resume bullet points

Pick 2-3 depending on the role you're targeting:

- Designed and shipped a full-stack Streamlit data application from requirements through production deployment in a 9-day sprint, processing 117,508+ real government records with a custom ETL pipeline
- Built an independently-tested analytics and insights engine decoupled from the UI layer, catching and fixing a statistically misleading auto-generated insight before it reached users
- Diagnosed and resolved a production deployment failure caused by a cloud-platform Python version mismatch, restoring a hung build to a successful public launch
- Conducted a systematic QA pass that identified and fixed three functional bugs — including a non-functional reset control and a data-integrity issue traced to an undocumented government coding scheme — before public release
- Sourced, cleaned, and integrated an official UK government reference dataset (STATS19 / DfT Local Authority lookup) to correct a data-quality gap discovered during testing

## Interview talking points

**"Tell me about a challenging bug you fixed."**
Two strong options:
1. The `.gitignore` invisible-character bug (Day 6) — a config file that *looked* completely correct on visual inspection but silently failed to work, diagnosed through elimination (checked negation rules, nested files, filename matching) before concluding it had to be encoding corruption, then fixed by rewriting the file directly via terminal instead of an editor.
2. The Python version deployment hang (Day 9) — a production build that hung with zero error output; traced the root cause (a brand-new Python version with no pre-built package for a pinned dependency) through research rather than guessing, and found the actual fix after the documented "standard" solution failed.

**"Tell me about a time you found a bug that wasn't obvious."**
The Reset button (Day 8): it ran without error, the page even visually refreshed — but it didn't actually clear any filters, because Streamlit persists widget state across reruns unless you explicitly clear it. This is a good example of testing *behavior*, not just *absence of errors*.

**"How do you approach scope creep?"**
Point to the PRD's explicit "Out of Scope" list from Day 1, and that it was still being actively referenced and defended on Day 6 (declining to compress the 10-day plan into fewer days even under time pressure, per an explicit build-log decision).

**"Describe your testing approach."**
Point to the separation between `analytics.py`/`insights.py` (pure logic, no Streamlit dependency) and the UI layer — this let core logic be tested with manual pandas cross-checks *before* any UI existed, so UI bugs and logic bugs were never conflated.

## Demo script (2-3 minutes)

1. **Open the live app.** "This is Road Accident Insight Explorer — it turns 117,000+ real UK government accident records into something anyone can explore, no SQL or Power BI needed."
2. **Point at the default view.** "On load, you already get the full picture: total accidents, an auto-generated insight panel, a hotspot map, and trend charts — all from real 2019 UK Department for Transport data."
3. **Apply a filter live.** Select Severity = "Fatal" and Weather = "Rain." "Watch the whole page update instantly — the map, the charts, and importantly, the insights panel recomputes new, statistically valid sentences for this exact selection, not just recycled ones."
4. **Point at the map.** "This isn't a scatter plot of dots — it's a density heatmap, so real hotspot clusters are visible at a glance, which is the actual point of a hotspot map."
5. **Mention the engineering, briefly.** "Everything here is free-tier — no database, no paid APIs, and the insights are computed with plain statistics, not an LLM call, which matters for a public-safety tool where you want the numbers to be exactly reproducible."
6. **Close.** "It's fully documented and open-source on GitHub, including the full build log and every bug I found and fixed along the way."

## Recommended GitHub topics

Add these under the repo's "About" section (gear icon → Topics):
`streamlit` `python` `data-visualization` `pandas` `plotly` `road-safety` `open-data` `dashboard` `data-analysis` `uk-government-data`

## Recommended repository metadata

- **Description:** "Interactive Streamlit app for exploring UK road accident data — hotspot map, trend charts, and auto-generated insights. Built in a 9-day sprint, deployed live."
- **Website field:** the live Streamlit app URL
- **Social preview image:** a screenshot of the app's default view (Settings → Social preview)
