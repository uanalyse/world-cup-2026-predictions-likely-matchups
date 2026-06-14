# World Cup 2026 predictions (detailed edition)

Private, detailed daily forecasts for the 2026 FIFA World Cup from
[uanalyse](https://uanalyse.co.uk). Every file is committed before the matches it
covers, so you can check how the model did once the results are in.

This repo carries the full daily forecast history plus the likely-matchups
breakdowns: for each knockout game it records which single team is most likely to
play in it, and which pair of teams is most likely to meet there.

## How to verify

Each daily commit is signed and timestamped by GitHub, and files under
`data/history/` are never edited after they land. So the git history is the
full, tamper-evident record of what we forecast and when.

```bash
git log --show-signature -- data/history/
```

## What's inside

`data/latest/` holds the most recent forecast. `data/history/<date>/` keeps
every past one. `data/likely_matchups/<index>/` holds the knockout-game
breakdowns.

### `tournament_probabilities.csv`

One row per team: the chance of reaching each stage.

| column | meaning |
| --- | --- |
| `snapshot_date` | the model snapshot the row comes from |
| `team` | team name |
| `group` | group |
| `prob_win_group` | win the group |
| `prob_runner_up` | finish second in the group |
| `prob_reach_round_of_32` | reach the round of 32 |
| `prob_reach_quarterfinals` | reach the quarterfinals |
| `prob_reach_semifinals` | reach the semifinals |
| `prob_reach_final` | reach the final |
| `prob_champion` | win the tournament |

### `match_predictions.csv`

Every scheduled fixture, with our result probabilities and expected goals.
Before the tournament that's the 72 group games. Knockout fixtures are added as
the bracket fills in and each tie becomes real.

| column | meaning |
| --- | --- |
| `snapshot_date` | the forecast snapshot the row comes from |
| `kickoff_date` | fixture date |
| `stage` | round |
| `fixture_type` | `group`, and `knockout` once those ties are set |
| `home_team`, `away_team` | the two sides |
| `prob_meeting` | chance this exact tie happens (`1.0` for scheduled fixtures) |
| `prob_home_win`, `prob_draw`, `prob_away_win` | 1X2 result probabilities |
| `exp_home_goals`, `exp_away_goals` | expected goals |

### `likely_matchups/<index>/`

`<index>` is the tournament ordinal of the next game still to be played, so each
folder reads as "the state before game `<index>`". As more games are played, new
index folders appear.

**`team_in_game.csv`** is every candidate team for each knockout game, with the
probability it plays in that game on either side, ordered most likely first.

| column | meaning |
| --- | --- |
| `game` | knockout match number (73 to 104) |
| `team` | team name |
| `probability` | chance this team plays in this game |
| `date`, `city`, `country` | fixture details |
| `stage` | round |

**`teams_in_game.csv`** is every candidate *pair* of teams for each knockout
game, ordered most likely first.

| column | meaning |
| --- | --- |
| `game` | knockout match number (73 to 104) |
| `team_a`, `team_b` | the candidate pair (side-agnostic) |
| `probability` | chance these two meet in this game |
| `date`, `city`, `country` | fixture details |
| `stage` | round |

## How it's made

The figures come from a pre-tournament Monte-Carlo forecast of 10,000 simulated
tournaments, run through our match model. We simulate the group stage, then
solve the knockout bracket exactly with dynamic programming, so the knockout
probabilities are computed rather than sampled. The live, round-by-round version
is on our [World Cup 2026 portal](https://uanalyse.co.uk/world-cup-2026). The
model itself is proprietary, so this repo holds the outputs only.

## Cadence

Updated about once a day through the tournament.

## Disclaimer

This is for information and entertainment. It isn't betting advice.
