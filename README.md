# Project Hyrox

Exploratory analysis of HYROX race results, split into two episodes. EP 1 compares the top 100 overall finishers across three 2026 events (Beijing, Bangkok II, and Chiba) to understand competitiveness, home-field advantage, and finish-time structure across cities and categories. EP 2 widens the scope to 13 Asia-Pacific races (singles categories only) to look at international participation and which nationalities post the fastest times.

## Contents

- `hyrox_result.csv` — EP 1 dataset: top 100 overall finishers per race category (all categories) for Beijing, Bangkok II, and Chiba, pulled from official HYROX results for each event.
- `hyrox_apac_singles_result.csv` — EP 2 dataset: top 100 overall finishers per singles category (Open Men, Open Women, Pro Men, Pro Women) across 13 Asia-Pacific races.
- `hyrox_comparison.ipynb` — analysis notebook containing data exploration, custom functions, and visualizations.

## Episodes at a glance

| Episode | Dataset | Races | Categories |
|---------|---------|-------|------------|
| EP 1 | `hyrox_result.csv` | 3 (Beijing, Bangkok II, Chiba) | All 9 categories |
| EP 2 | `hyrox_apac_singles_result.csv` | 13 | 4 singles categories |

## Dataset

Each row represents one athlete's result within a specific race and category (not a single global leaderboard — rankings are scoped per `race_category`). Both CSV files share the same columns.

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

**Note:** For doubles categories (EP 1 only), pairs with different nationalities will have slash "/" (e.g. CHN/JPN)

## Categories covered

**EP 1 (all categories)**
- Open Men / Open Women
- Pro Men / Pro Women
- Doubles Men / Doubles Women / Doubles Mixed
- Pro Doubles Men / Pro Doubles Women

**EP 2 (singles only)**
- Open Men / Open Women
- Pro Men / Pro Women

## Races covered

**EP 1:** Beijing 2026, Bangkok II 2026, Chiba 2026

**EP 2:** 13 races, listed from most recent to oldest:

- Beijing 2026
- Perth 2026
- Shenzhen 2026
- Bangkok II 2026
- Chiba 2026
- Sydney 2026
- Jakarta 2026
- Incheon 2026
- Hong Kong 2026
- Singapore 2026
- Bangkok I 2026
- Taipei 2026
- Osaka 2026

---

# EP 1: Beijing vs Bangkok II vs Chiba (all categories)

## Key questions explored

- **Competitiveness comparison across cities** — how do finish times at a given rank (1st, 25th, 50th, 75th, 100th) compare between Beijing, Bangkok II, and Chiba?
- **Home-field advantage** — what proportion of each city's top 100 is made up of home-nationality athletes, and how does this vary by category and rank tier?
- **Field depth and structure** — how does the shape of the rank-vs-finish-time curve compare across categories and cities (e.g. elite-tier gaps, mid-pack density, back-of-pack drop-offs)?

## Example findings

- Beijing shows consistently high and stable home-athlete representation across nearly all rank tiers and categories, while Bangkok's international representation is highest in the middle of the field (ranks ~26–75) rather than at the very top or bottom.
- Pro categories show the widest spread in competitiveness: Beijing's Pro fields are notably deeper (flatter finish-time curve) than Bangkok's or Chiba's, particularly in the women's Pro categories, which show sharp late-field time spikes.
- Despite differences in pace and home dominance, the overall *shape* of the rank-vs-time curve (fast top tier → flatter middle → slight late-race increase) is broadly consistent across cities and categories.

---

# EP 2: Asia-Pacific nationalities across 13 races (singles only)

## Key questions explored

- **International competition** — which foreign nationalities appear most often in the Top 100 across all 13 races?
- **Fastest nationalities** — which nationalities post the fastest finish times when each nationality's 100 best results are compared?

## Methodology

### Part 1: Ranking by international competition (Top 100 finish as foreign athlete)

This analysis finds which foreign nationalities appear most often in the Top 100 overall of each singles category, pooled across all 13 races in the dataset.

1. **Define "home" for each race.** Each host country is mapped to its nationality code (e.g. China → `CHN`, Thailand → `THA`, Japan → `JPN`). An athlete counts as a home athlete when their `nationality` matches the host country's code for that race. A check also flags any host country missing from the map, since an unmapped country would wrongly count its whole field as foreign.
2. **Filter to singles categories.** Only Open Men, Open Women, Pro Men and Pro Women are kept.
3. **Remove home athletes.** Only non-home athletes remain, so the counts show who travels to race rather than who lives locally.
4. **Count nationalities per category.** Non-home athletes are grouped by category and nationality, then counted. The counts are pooled across all races, so the same nationality is summed across every city it appears in.
5. **Rank and trim.** Within each category, nationalities are ranked by count (1 = most frequent), and the top 8 per category are kept.

**Note:** The same nationality can be home in one race and non-home in another. For example, a `CHN` athlete is excluded in Beijing but counted as a traveling athlete in Osaka.

### Part 2: Ranking by the best finish time of each nationality across 13 races

This analysis finds which nationalities post the fastest finish times in the Top 100 overall of each singles category, pooled across all 13 races. Pooling many venues and dates gives major nationalities enough data to compare, and averaging 100 times rewards depth rather than one star athlete. However, the data has no athlete names, so finish times are not unique and the same athlete can appear several times in one nationality's 100 best times, which means the ranking measures the best performances rather than the best athletes. The results are also shaped by who is in the data: only Top 100 finishers are included, host countries are over-represented because locals enter their home races in large numbers, and course, weather and field strength differ by race, so this is not a true global ranking of the best nationalities in HYROX.

1. **Pool all races.** The Top 100 results of all 13 races are combined into one dataset. Home athletes stay in, since the question is which nationalities are fastest overall, not only which ones travel.
2. **Filter to singles categories.** Each category is ranked separately because Pro uses heavier weights and men and women compete in different divisions.
3. **Convert finish time to a number.** `finish_time` is converted from `H:MM:SS` to total minutes so it can be sorted and averaged.
4. **Collect the 100 best times per nationality.** Within each category, results are grouped by nationality, sorted from fastest to slowest, and the 100 fastest are kept, along with each athlete's age group. Nationalities with fewer than 100 results keep all their rows.
5. **Summarize and rank.** For each nationality's 100 best times, the mean (or median) is calculated, and nationalities are ranked from lowest (fastest) to highest.

## Example findings

- South Korea and Hong Kong are the strongest Asian traveling nationalities in the Top 100, while Great Britain and Australia are the strongest non-Asian representatives.
- In Open Men, South Korea has the fastest 100 best times of the nationalities compared, followed by China, Japan and Hong Kong, while Thailand has fewer than 100 results and a steeper climb in times.

---

## Usage

```bash
pip install pandas matplotlib
jupyter notebook hyrox_comparison.ipynb
```


- Datasets are limited to the top 100 overall finishers per category, so they do not capture full field size or participation rates.
- The data has no athlete names, so repeated athletes cannot be identified or removed (relevant to EP 2 in particular).
- Some very high back-of-pack finish times (e.g. in Pro Women/Pro Doubles Women) should be sanity-checked against the source results, as they may reflect time-cap or DNF-adjacent entries rather than typical race pace.
- EP 2 covers Asia-Pacific races only, so results should not be read as a global ranking of nationalities.
