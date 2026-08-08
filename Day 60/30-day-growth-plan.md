# 30-Day Growth Plan — Road Accident Insight Explorer

A realistic, one-milestone-per-day roadmap taking the v1.0.0 MVP toward the "Next 3 months" goals in `future-scope.md`. Each day builds directly on the previous one — no day requires skipping ahead. Roughly organized into four weeks with a lighter final week for polish and reflection.

## Week 1 — Multi-year data foundation

1. Download DfT accident data for 2020 and 2021; confirm the schema matches 2019's structure (same STATS19 columns).
2. Extend `data_cleaning.py` to accept multiple input years and add a `year` field to the schema.
3. Re-run the cleaning pipeline across all three years; validate combined row counts and check for any new category codes not in the existing lookups.
4. Update `SCHEMA.md` to reflect the new `year` field and multi-year scope.
5. Add a `Year` filter to the sidebar, wired the same way as the existing filters.
6. Test all existing insights/charts still work correctly with 3x the data volume; check performance.
7. Commit, push, deploy the updated multi-year version; update `README.md`'s dataset description.

## Week 2 — Year-over-year comparison

8. Design (on paper first) what a "compare two time periods" view should show — sketch before coding, same discipline as the original PRD.
9. Build a new `compare_periods()` function in `analytics.py`, fully unit-testable, no UI yet (same Day 5 pattern: logic before UI).
10. Write 3-4 test scenarios for it manually, same rigor as Day 5's testing.
11. Add a "Compare" mode toggle to the UI.
12. Build the comparison visualization (e.g., side-by-side bar charts or a delta view).
13. Add 1-2 new auto-generated insights specific to comparison mode (e.g., "Accidents in this region dropped 12% from 2019 to 2021").
14. Full manual test pass on the new feature; document any bugs found in `TESTING.md`, same format as Day 8.

## Week 3 — Data portability + shareable views

15. Add a "Download this selection as CSV" button using `st.download_button`.
16. Test the download works correctly across several filter combinations, including edge cases (very large selection, empty selection).
17. Implement shareable filter state via `st.query_params` — encode active filters into the URL.
18. Test that a shared URL correctly restores the exact filter state for a new visitor.
19. Add a "Copy shareable link" button to the UI.
20. Write a short usage guide for both new features and add it to `docs/`.
21. Deploy the updated version; smoke-test the live deployment exactly like Day 9.

## Week 4 — Accessibility + credibility pass

22. Do a real keyboard-only navigation test of the entire app (no mouse) — document every point of friction.
23. Do a real screen reader test (Windows Narrator or NVDA, free) — document what's inaccessible.
24. Fix the highest-impact 2-3 accessibility issues found.
25. Add a formal "Methodology" page/section explaining exactly how insights are computed (builds credibility for a public-safety tool).
26. Review and update all `docs/` files for accuracy against the now-larger feature set.
27. Get one honest outside opinion — show the app to someone outside this project and watch them use it without guidance, exactly like the original PRD's usability bar.

## Final week — Consolidate and reflect

28. Fix whatever the outside tester struggled with.
29. Tag a `v1.1.0` release with proper release notes summarizing the month's additions.
30. Update `challenge-retrospective.md` and `PORTFOLIO.md` with what changed since v1.0.0 — the growth is part of the portfolio story too.
