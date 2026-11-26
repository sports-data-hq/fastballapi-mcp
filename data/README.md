# FastballAPI season pack

The last completed season of baseball as CSV, free to use under the [FastballAPI Sample Data Licence](../DATA-LICENSE.md). Use it in anything you build, credit FastballAPI, and don't republish the files themselves.

**Download:** the latest `fastballapi-season-pack.zip` from this repository's [season-pack release](https://github.com/sports-data-hq/fastballapi-mcp/releases/tag/season-pack). `preview/` holds the first 100 rows of a few files.

Seasons: 2026. Season naming: the calendar year. Pitching shows runs allowed per 9 innings (RA9), not ERA. Competitions, by their API id: mlb, ncaabase. No odds, props or play-by-play: those, every earlier season and live data are on the API, free to start at https://fastballapi.com/signup?utm_source=github&utm_medium=pack&utm_campaign=fastballapi-pack.

## Files

| File | Rows |
|---|---|
| `fastballapi-mlb-2026-batting.csv` | 739 |
| `fastballapi-mlb-2026-games.csv` | 2,469 |
| `fastballapi-mlb-2026-pitching.csv` | 1,003 |
| `fastballapi-mlb-2026-standings.csv` | 30 |
| `fastballapi-mlb-2026-teams.csv` | 30 |
| `fastballapi-ncaabase-2026-batting.csv` | 4,052 |
| `fastballapi-ncaabase-2026-games.csv` | 5,363 |
| `fastballapi-ncaabase-2026-pitching.csv` | 4,081 |
| `fastballapi-ncaabase-2026-standings.csv` | 248 |
| `fastballapi-ncaabase-2026-teams.csv` | 322 |

## Columns

- **batting**: `league`, `season`, `player`, `team`, `pa`, `ab`, `h`, `doubles`, `triples`, `hr`, `bb`, `hbp`, `so`, `sf`, `sh`, `avg`, `obp`, `slg`, `run_value`
- **games**: `league`, `season`, `stage`, `round`, `date`, `home_team`, `away_team`, `home_score`, `away_score`, `home_hits`, `away_hits`, `home_errors`, `away_errors`, `innings`, `venue`
- **pitching**: `league`, `season`, `pitcher`, `team`, `batters_faced`, `outs`, `innings_pitched`, `h`, `hr`, `bb`, `hbp`, `so`, `runs_allowed`, `ra9`, `whip`, `run_value_saved`
- **standings**: `league`, `season`, `conference`, `team`, `games`, `wins`, `losses`, `win_pct`, `home_wins`, `home_losses`, `away_wins`, `away_losses`, `runs_scored`, `runs_allowed`, `run_diff`, `pythag_win_pct`
- **teams**: `league`, `season`, `team`, `elo_end_of_season`, `plate_appearances`, `home_runs`, `batting_run_value`

Credit line for anything you publish: **Data: FastballAPI (fastballapi.com)**
