# Data Schema — Road Accident Insight Explorer

**Status:** Approved Day 2 design
**Storage format:** Single denormalized table, stored as Parquet (`data/processed/cleaned_accidents.parquet`), CSV fallback for portability.

## Why one flat table, not a relational schema

This app has one read-heavy analytical workload with no writes, no user-submitted data, and no relationships to maintain — it's a classic single-fact-table BI dataset, the same shape you'd already recognize from Power BI or a flat Excel export. A multi-table relational schema would require joins on every filter change with no benefit, since there's nothing to normalize (no repeating entities like "officer" or "vehicle" that need their own table in v1.0). If a future scope item (e.g., multi-dataset merging) requires it, this can be revisited — but it would be a deliberate scope change, not a Day 2 default.

---

## Table: `cleaned_accidents`

| Column | Type | Nullable | Description | Source |
|---|---|---|---|---|
| `accident_id` | string | No | Unique identifier for the accident record | Raw dataset (or generated if absent) |
| `date` | date | No | Date of the accident (YYYY-MM-DD) | Raw dataset, parsed |
| `time` | time | Yes | Time of day of the accident (HH:MM) | Raw dataset, parsed |
| `hour` | int (0–23) | Yes | Hour of day, derived from `time` | Derived |
| `day_of_week` | string (Mon–Sun) | No | Day of week, derived from `date` | Derived |
| `month` | string | No | Month name, derived from `date` | Derived |
| `latitude` | float | Yes | Latitude coordinate | Raw dataset |
| `longitude` | float | Yes | Longitude coordinate | Raw dataset |
| `region` | string | No | Region/city/district name | Raw dataset, standardized |
| `severity` | categorical (`Slight`, `Serious`, `Fatal`) | No | Accident severity level | Raw dataset, standardized to 3 fixed categories |
| `weather_condition` | categorical | No | Weather at time of accident (e.g. `Clear`, `Rain`, `Fog`, `Snow`) | Raw dataset, standardized |
| `road_type` | categorical | Yes | Road/junction type (e.g. `Roundabout`, `Single carriageway`, `Junction`) | Raw dataset, standardized (nullable if not present in chosen dataset) |
| `num_casualties` | int | Yes | Number of casualties, defaults to 0 if absent in source | Raw dataset |

### Constraints & validation rules (enforced during Day 3–4 cleaning)

- `date` must parse to a valid date; rows that fail to parse are dropped and logged.
- `severity` must map to exactly one of the 3 fixed categories — any unrecognized raw value is mapped to `Unknown` rather than dropped, so the row is still usable in non-severity-filtered views.
- `latitude`/`longitude` are validated to fall within plausible bounds for the source country/region; out-of-range values are set to null (row is excluded from the map, but retained for trend/severity charts).
- `weather_condition` and `road_type` raw category strings are consolidated (e.g. `"rain"`, `"Rain"`, `"RAINY"` → `"Rain"`) during cleaning.
- Duplicate `accident_id` values are deduplicated, keeping the first occurrence.

---

## Schema validation against PRD user stories

| User story | Supported by | ✅ |
|---|---|---|
| City planner filters by time of day | `hour`, `time` | ✅ |
| Police analyst sees hotspot map | `latitude`, `longitude` | ✅ |
| Student researcher filters by weather, studies severity relationship | `weather_condition` × `severity` | ✅ |
| Curious citizen reads plain-language insights | All fields feed `insights.py` aggregations | ✅ |
| Any user filters by region | `region` | ✅ |
| Trend charts by hour of day and day of week (FR-4) | `hour`, `day_of_week` | ✅ |
| Severity breakdown (FR-5) | `severity` | ✅ |

All required fields from PRD Section 9 ("Data Requirements") are present. No gaps identified.

---

## Sample row

```json
{
  "accident_id": "A0001234",
  "date": "2023-06-14",
  "time": "17:45",
  "hour": 17,
  "day_of_week": "Wed",
  "month": "June",
  "latitude": 28.6139,
  "longitude": 77.2090,
  "region": "New Delhi",
  "severity": "Serious",
  "weather_condition": "Rain",
  "road_type": "Junction",
  "num_casualties": 1
}
```

*(Illustrative only — real values depend on the dataset selected on Day 3.)*
