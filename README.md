# FastballAPI MCP server

[![MCP Registry](https://img.shields.io/badge/MCP%20Registry-listed-0b7285?style=flat-square)](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.noahowsh/fastballapi)

**The baseball data API.** Pitch-by-pitch data with pitch type, velocity and location, every plate appearance with base-out state and the batted ball, box scores, standings, our own ratings and run values, sportsbook lines by book and MLB player prop lines. MLB since 2015 plus the NCAA Division I games we cover. REST API and MCP server. 500 free credits every month, no card.

Website: https://fastballapi.com?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp · Docs: https://fastballapi.com/docs?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp · Get a key: https://fastballapi.com/signup?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp

```
You:    "What is Gerrit Cole's whiff rate by pitch type this season?"
Claude: [calls get_pitch_mix]

You:    "How often did Aaron Judge homer on a 3-2 count in 2025?"
Claude: [calls get_plate_appearances]

You:    "How often has Shohei Ohtani gone over his total bases line since 2023?"
Claude: [calls get_prop_history]
```


## Run it locally (stdio)

Most clients connect to `https://mcp.fastballapi.com/mcp` directly. For a client that launches a local process instead:

```bash
git clone https://github.com/sports-data-hq/fastballapi-mcp.git && cd fastballapi-mcp && npm install
FASTBALLAPI_API_KEY=your_key node index.js
```

```json
{ "mcpServers": { "fastballapi": { "command": "node", "args": ["/path/to/fastballapi-mcp/index.js"], "env": { "FASTBALLAPI_API_KEY": "your_key" } } } }
```

It forwards to the hosted server with your key, so every tool and credit cost is the same.

## Skills

7 Claude skills for baseball ship in this repo, plus 15 analytics skills for models, backtests, calibration and comparing a model with the market. Every number in an answer comes from a live FastballAPI tool call, and odds are information only: no stakes, no picks.

Claude Code:

```
/plugin marketplace add sports-data-hq/fastballapi-mcp
/plugin install mlb-data@fastballapi
/plugin install sports-analytics-core@fastballapi
```

Codex: `codex plugin marketplace add sports-data-hq/fastballapi-mcp`. Any agent that reads skills: `npx skills add sports-data-hq/fastballapi-mcp`.

| Skill | What it does |
|---|---|
| `mlb-batter-spray-profile` | Where a batter hits the ball: trajectory and field direction, with hit rates and counts |
| `mlb-game-preview` | One-page preview: records, run differential, our Elo, the starters' arsenals, injuries and the line |
| `mlb-pitcher-skill-change` | Has a pitcher changed? Pitch usage, velocity and whiff rate by pitch type across games or seasons |
| `mlb-player-props-research` | Strikeout, outs, hits, home run and total bases lines by sportsbook, with the record against the line and workload |
| `mlb-regression-candidates` | Who is due to regress: results against steadier rates like BABIP, strikeouts, walks and home runs per fly |
| `mlb-streaming-planner` | Fantasy streaming plan for the week: probable starters, opponents and park context |
| `mlb-win-prob-swings` | The biggest win-probability swings of a game, plate appearance by plate appearance |

The analytics skills: `backtesting`, `data-pipeline`, `elo-engineering`, `feature-engineering`, `market-vs-model`, `model-building`, `nl-to-query`, `odds-market-analysis`, `prediction-tracker`, `probability-calibration`, `season-pack-offline`, `sports-data-preflight`, `sports-skill-router`, `visualization`, `walk-forward-validation`.

All nine sports' skills live in [sports-data-hq/sportsdatahq-skills](https://github.com/sports-data-hq/sportsdatahq-skills). The copies here are synced from there, so please open issues and pull requests there.

## Python

The official Python client is on PyPI: [fastballapi](https://pypi.org/project/fastballapi/).

```bash
pip install fastballapi
export FASTBALLAPI_API_KEY=sk_live_...
```

```python
from fastballapi import Client, to_dataframe

client = Client()

props = client.get_player_props(
    game_id="401815991",
    market="batter_home_runs",
)
df = to_dataframe(props, key="props")
print(df[["player_name", "bookmaker", "line", "over_odds", "under_odds"]].head(10))
```

Typed errors for a bad key, no credits left, not found and rate limits, automatic retries, and `to_dataframe` for pandas.

## Quick start

This is a hosted (remote) MCP server over Streamable HTTP. There is nothing to install.

**Claude Code**

```bash
claude mcp add --transport http fastballapi "https://mcp.fastballapi.com/mcp?key=YOUR_API_KEY"
```

**Claude Desktop, Cursor and other clients**

```json
{
  "mcpServers": {
    "fastballapi": {
      "url": "https://mcp.fastballapi.com/mcp?key=YOUR_API_KEY"
    }
  }
}
```

The key can also be sent as an `x-api-key` header or `Authorization: Bearer`. Connecting and listing tools works without a key; calling a tool needs one.

**REST**

```bash
curl -X POST https://mcp.fastballapi.com/v1/list_teams -H "x-api-key: YOUR_API_KEY"
```

Every tool is `POST /v1/{tool}` with a JSON body.

## Examples

Runnable Python (standard library only) and Node 18+ (no dependencies) examples live in [`examples/`](examples/):

- [`examples/python/quickstart.py`](examples/python/quickstart.py) and [`examples/node/quickstart.mjs`](examples/node/quickstart.mjs): schedule plus a headline call.
- [`examples/python/mlb_pitch_mix_and_props.py`](examples/python/mlb_pitch_mix_and_props.py): A pitcher's arsenal (usage, velocity, whiff rate and ball rate per pitch type) followed by his strikeout prop lines against what he actually threw, game by game.

```bash
export FASTBALLAPI_API_KEY=your_key
python3 examples/python/quickstart.py
node examples/node/quickstart.mjs
```

See [`examples/README.md`](examples/README.md) for details. Get a free key (500 credits a month, no card) at https://fastballapi.com/signup?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp.

## Starter projects

- [mlb-pitch-mix](https://github.com/sports-data-hq/mlb-pitch-mix): a pitcher's arsenal, what he throws in each count and his velocity by inning, in one Python script.

## Use it in

- **Claude, ChatGPT, Cursor, VS Code, Gemini CLI, Grok, Perplexity, Le Chat:** add `https://mcp.fastballapi.com/mcp` as a remote MCP server. Step-by-step for each: https://fastballapi.com/mcp?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp
- **Gemini CLI:** `gemini extensions install https://github.com/sports-data-hq/fastballapi-mcp`
- **Cursor:** the [SportsDataHQ plugins for Cursor](https://github.com/sports-data-hq/sportsdatahq-cursor-plugins)
- **Google Sheets:** `=SDHQ("fastballapi","list_teams")` fills a live table: https://sportsdatahq.com/sheets?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp
- **REST:** `POST https://mcp.fastballapi.com/v1/<tool>` with an `x-api-key` header: https://fastballapi.com/docs?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp

## Tools (23)

| Tool | What it returns |
|------|-----------------|
| `list_teams` | Team ids, abbreviations and colours for a season |
| `search_players` | Find players by name |
| `get_games` | Games with runs, hits and errors by season, date, range or team |
| `get_schedule` | Upcoming games with the median line across sportsbooks |
| `get_standings` | Standings from every final game, with run differential and Pythagorean record |
| `get_injuries` | The latest injury report |
| `get_ratings` | Our team Elo ratings, ranked |
| `get_game` | One game: every batting and pitching line, each book's line, win probability |
| `get_player` | Season batting (AVG, OBP, SLG, OPS, RE24) and pitching (ERA, WHIP, K/9) |
| `get_player_games` | A player's full batting and pitching game log |
| `get_leaders` | Season leaders among qualified batters and pitchers |
| `get_team_stats` | Runs for and against, team batting and opponents' batting |
| `get_pitch_mix` | A pitcher's arsenal: usage, velocity, whiff rate per pitch type |
| `get_odds` | Every sportsbook's moneyline, run line and total for a game (5) or a day (10) |
| `get_plate_appearances` | Plate-appearance play-by-play with base-out state, batted ball, win probability, RE24 |
| `get_pitches` | Pitch-by-pitch: count, type, velocity, location, result |
| `get_spray_chart` | Where a batter or team hits the ball, by trajectory and field |
| `get_line_movement` | Snapshots of each sportsbook's line before first pitch |
| `get_player_props` | Player prop lines by sportsbook: the closing line (one pre-game capture per line before October 2026) |
| `get_prop_history` | A player's prop lines against the result, game by game |
| `get_prop_movement` | How one player's prop line moved before a game, every capture by sportsbook |
| `get_market_odds` | Opening and closing lines by sportsbook for the first-5-innings and first-inning markets (history from May 2023) |
| `get_alt_lines` | Alternate run lines and totals by sportsbook: every point on each book's ladder, opening and closing, from 2023 |

Leagues: mlb (default), ncaabase (NCAA Division I games we cover; pitch tools, spray charts and injuries are not offered for college).

## Data

- MLB games since 2015 with pitch-by-pitch data (every pitch from 2018; 2015 to 2017 have about 98 to 99% of each season's pitches)
- Every plate appearance with base-out state, batted ball, win probability and RE24
- Per-sportsbook moneylines, run lines and totals: 4 to 13 books per game from 2015 to 2023 (about 4 in 2015, 10 in 2016 and 2017, 7 to 8 in 2018 to 2020, 12 in 2021 to 2023), usually one book since 2024, plus opening and closing lines since July 2020 (a median of 7 books in 2021 to 2023 and 8 since 2024); pre-game line snapshots from launch: once a day, and every hour for games in the next two days
- MLB player prop lines by sportsbook (hits, home runs, total bases, RBI, pitcher strikeouts, pitcher outs) from May 2023, with the result against each line (`get_player_props`, `get_prop_history`); none for college
- NCAA Division I college baseball for the games we cover (not every Division I game)

Coverage by season, read live from the database: https://fastballapi.com/docs?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp

## Useful pages

- [Docs](https://fastballapi.com/docs?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): every endpoint with parameters and a live example response
- [Data atlas](https://fastballapi.com/docs/atlas?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): what exists, which tool exposes it, coverage by season
- [MLB odds](https://fastballapi.com/odds?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): upcoming moneylines, run lines and totals by sportsbook
- [MLB player props today](https://fastballapi.com/props?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): hits, home runs, total bases, RBIs and pitcher strikeouts, best price per player across books
- [Guides](https://fastballapi.com/guides?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): pitch data for a model, pitch mix and whiff rate, Pythagorean standings, run value with runners in scoring position, spray charts
- [Data](https://fastballapi.com/data?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): [pitch-by-pitch](https://fastballapi.com/data/mlb-pitch-by-pitch?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp), [spray charts](https://fastballapi.com/data/mlb-spray-chart-data?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp), [odds history](https://fastballapi.com/data/mlb-odds-history?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp), [player props](https://fastballapi.com/data/player-props?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp)
- [College baseball](https://fastballapi.com/college-baseball?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): the NCAA Division I games we cover
- [Pricing](https://fastballapi.com/pricing?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp): free tier and paid plans
- [Glossary](https://fastballapi.com/glossary?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp)

## Pricing

500 free credits every month, no card. Plans: Starter $19/mo (10,000 credits), Pro $49/mo (30,000), Scale $149/mo (125,000), Business $299/mo (300,000). Wallet top-ups from $5, never expire. https://fastballapi.com/pricing?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp

## Attribution

Ratings and run values are ours. Not affiliated with MLB, the NCAA or any team.

## License

MIT for the contents of this repository. Data terms as above.

## Use with Codex

The SportsDataHQ skills marketplace also works in Codex (it reads `.agents/plugins/marketplace.json`):

```bash
codex plugin marketplace add sports-data-hq/sportsdatahq-skills
```

Install `mlb-data`, which bundles the baseball skills and the FastballAPI MCP server.

## Use with Gemini CLI

```bash
export FASTBALLAPI_API_KEY=your_key
gemini extensions install https://github.com/sports-data-hq/fastballapi-mcp
```

The extension adds the FastballAPI MCP server (https://mcp.fastballapi.com/mcp) and sends the key in the `x-api-key` header. Get a free key (500 credits a month) at https://fastballapi.com/signup?utm_source=github&utm_medium=readme&utm_campaign=fastballapi-mcp.
