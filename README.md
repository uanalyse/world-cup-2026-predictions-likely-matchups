# World Cup 2026 likely matchups

Private likely-matchups forecasts for the 2026 FIFA World Cup from
[uanalyse](https://uanalyse.co.uk). For each knockout game we record which single
team is most likely to play in it, and which pair of teams is most likely to meet
there. Files are committed before the matches they cover, so you can check how the
model did once the results are in.

## How to verify

Each commit is signed and timestamped by GitHub, and the files are never edited
after they land. So the git history is the full, tamper-evident record of what we
forecast and when.

```bash
git log --show-signature -- data/likely_matchups/
```

## What's inside

`data/likely_matchups/<index>/` holds the knockout-game breakdowns. `<index>` is
the tournament ordinal of the next game still to be played, so each folder reads
as "the state before game `<index>`". As more games are played, new index folders
appear.

### `team_in_game.csv`

Every candidate team for each knockout game, with the probability it plays in that
game on either side, ordered most likely first.

| column | meaning |
| --- | --- |
| `game` | knockout match number (73 to 104) |
| `team` | team name |
| `probability` | chance this team plays in this game |
| `date`, `city`, `country` | fixture details |
| `stage` | round |

### `teams_in_game.csv`

Every candidate *pair* of teams for each knockout game, ordered most likely first.

| column | meaning |
| --- | --- |
| `game` | knockout match number (73 to 104) |
| `team_a`, `team_b` | the candidate pair (side-agnostic) |
| `probability` | chance these two meet in this game |
| `date`, `city`, `country` | fixture details |
| `stage` | round |

## How it's made

The figures come from a pre-tournament Monte-Carlo forecast of 10,000 simulated
tournaments, run through our match model. We simulate the group stage, then solve
the knockout bracket exactly with dynamic programming, so the knockout
probabilities are computed rather than sampled. The live, round-by-round version
is on our [World Cup 2026 portal](https://uanalyse.co.uk/world-cup-2026). The
model itself is proprietary, so this repo holds the outputs only.

## Cadence

Updated about once a day through the tournament.

## Disclaimer

This is for information and entertainment. It isn't betting advice.
