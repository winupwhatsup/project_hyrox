# Project Hyrox

Exploratory analysis of HYROX race results, comparing the top 100 overall finishers across multiple 2026 events (Beijing, Bangkok, and Chiba) to understand competitiveness, home-field advantage, and finish-time structure across cities, categories, and age groups.

## Contents

- `hyrox_result.csv` — raw dataset of top 100 overall finishers per race category, pulled from official HYROX results for each event.
- `notebook.ipynb` — analysis notebook containing data exploration, custom functions, and visualizations.

## Dataset

Each row represents one athlete's result within a specific race and category (not a single global leaderboard — rankings are scoped per `race_category`).

| Column          | Description                                                          |
|-----------------|-----------------------------------------------------------------------|
| `race_name`     | Event name and year, e.g. `Beijing 2026`                              |
| `race_country`  | Host country of the event, e.g. `China`                               |
| `race_category` | Competition category, e.g. `Open Men`, `Pro Doubles Women`            |
| `age_category`  | Age group of the athlete (individual events only)                     |
| `overall_rank`  | Athlete's rank within their `race_category` (1–100)                   |
| `ag_rank`       | Athlete's rank within their age group and `race_category`             |
| `finish_time`   | Finish time, formatted `H:MM:SS`                                      |
| `nationality`   | Athlete's nationality, as an IOC-style 3-letter code (e.g. `CHN`)      |

**Note:** For doubles categories, nationality reflects one representative nationality per team, not both partners individually.

## Categories covered

- Open Men / Open Women
- Pro Men / Pro Women
- Doubles Men / Doubles Women / Doubles Mixed
- Pro Doubles Men / Pro Doubles Women

## Key questions explored

- **Competitiveness comparison across cities** — how do finish times at a given rank (1st, 25th, 50th, 75th, 100th) compare between Beijing, Bangkok, and Chiba?
- **Home-field advantage** — what proportion of each city's top 100 is made up of home-nationality athletes, and how does this vary by category and rank tier?
- **Traveling athlete patterns** — which non-home nationalities appear most frequently in the top 100, and are the same nationalities showing up across multiple cities?
- **Field depth and structure** — how does the shape of the rank-vs-finish-time curve compare across categories and cities (e.g. elite-tier gaps, mid-pack density, back-of-pack drop-offs)?

## Example findings

- Beijing shows consistently high and stable home-athlete representation across nearly all rank tiers and categories, while Bangkok's international representation is highest in the middle of the field (ranks ~26–75) rather than at the very top or bottom.
- Pro categories show the widest spread in competitiveness: Beijing's Pro fields are notably deeper (flatter finish-time curve) than Bangkok's or Chiba's, particularly in the women's Pro categories, which show sharp late-field time spikes.
- Despite differences in pace and home dominance, the overall *shape* of the rank-vs-time curve (fast top tier → flatter middle → slight late-race increase) is broadly consistent across cities and categories.

## Requirements
