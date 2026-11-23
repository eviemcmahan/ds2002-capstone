# ACC Reality Check

**DS2002 · Fall 2026 · Capstone · Evie McMahan, Honor Dold, Julia McPhillips**

Which ACC men's basketball teams are as good as their records, and which ones have built their records on easy opponents?

## Responsibilities

| Area | Owner | What they are responsible for |
|------|-------|-------------------------------|
| Ratings CSV and name crosswalk | Evie | Torvik pulls and twice-weekly snapshots, ESPN to Torvik name matching, crosswalk assertions |
| ESPN pulls and box scores | Honor | Scoreboard and summary pulls, saved raw JSON, UTC to Eastern dates, `team_games` and `player_games`, possessions |
| SQLite schema and queries | Julia | `acc.db` tables and primary keys, safe rerun load, SQL for Q1 to Q6, charts 1 to 3, decision log |
| API and PythonAnywhere | Evie | `api/main.py`, deployment, five routes with `?` placeholders and 404/400 handling |
| Chart 4 | Julia | Must be someone who did not write the API; calls the live API with `requests` |
| Presentation | Honor | Build the presentation file |

## Summary


## Links

| What | Link |
|------|------|
| Live API | https://TODO.pythonanywhere.com |
| API docs (all five routes) | https://TODO.pythonanywhere.com/docs |
| Presentation video (8 min or less) | TODO |
| Slides | TODO (or `slides/`) |

## Data

| Source | Address | Pulled |
|--------|---------|--------|
| Bart Torvik team ratings (CSV, one snapshot per pull) | `https://barttorvik.com/2027_team_results.csv` | TODO: list each snapshot date |
| ESPN scoreboard (JSON, `groups=2`, one request per date) | `https://site.api.espn.com/apis/site/v2/sports/basketball/mens-college-basketball/scoreboard` | TODO: Nov 1 through TODO |
| ESPN game summary / box scores (JSON, one per finished game) | `https://site.api.espn.com/apis/site/v2/sports/basketball/mens-college-basketball/summary` | TODO |

External API used: **ESPN public site API**, base URL: `https://site.api.espn.com/apis/site/v2/sports/basketball/mens-college-basketball`

## Repository layout

```
.
├── README.md
├── notebook.ipynb        Final team notebook (pipeline, SQL, charts, decision log, reflection)
├── acc.db                SQLite database that the answers and the API came from
├── api/
│   ├── main.py           FastAPI app deployed on PythonAnywhere (reads acc.db only)
│   └── requirements.txt  Packages for the API
├── data/raw/             Saved raw responses (Torvik CSV snapshots, ESPN JSON)
```

## How to run

1. Clone the repo and install the packages: `pip install -r requirements.txt`
2. Open `notebook.ipynb` in Jupyter, Kaggle or Colab.
3. Run all cells top to bottom (Restart kernel, then Run All). The notebook reads saved copies in `data/raw/` when they exist and only calls the sources for data it does not have, so a rerun does not create duplicates in `acc.db`.

## Database (`acc.db`)

| Table | One row per | Purpose |
|-------|-------------|---------|
| `teams` | team | Every team in the games, with ESPN id, ESPN name, Torvik name, ACC flag |
| `team_crosswalk` | source name | Maps each ESPN or Torvik spelling to a team |
| `ratings` | team per snapshot date | Torvik values on the day each snapshot was downloaded |
| `games` | game | Eastern date, UTC start time, home and away teams, scores, neutral-site flag |
| `team_games` | team per game | Points for and against, margin, box score totals, estimated possessions |
| `player_games` | player per game | Minutes, points, rebounds, assists, steals, blocks, turnovers |

All tables use primary keys and are loaded with `INSERT OR REPLACE`, so rerunning the load leaves every row count unchanged.

## API routes

Base URL: `https://TODO.pythonanywhere.com`

| Route | Returns |
|-------|---------|
| `/teams` | All 18 ACC teams with record, efficiency margin, average opponent Barthag, and a short label |
| `/teams/{team}` | One team's summary and last five games (404 for an unknown team) |
| `/games?date=YYYY-MM-DD` | Every game on that Eastern date |
| `/matchup?a=...&b=...` | Both teams' current ratings, the favorite, and the efficiency margin gap |
| `/leaders?stat=...` | ACC players ranked by points, rebounds, assists, steals, or blocks per game (400 for any other stat) |

## Key findings

TODO: each finding with its number and the number of games it rests on.

- Finding 1 (Q1, record vs. rating):
- Finding 2 (Q2, strength of schedule):
- Finding 3 (Q6, does the rating predict the winner?):

## Recommendation

TODO: which ACC teams have earned their records and which have not, based on the data.

## Reflection

TODO: Reflection questions 1-5.