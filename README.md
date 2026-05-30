# ORACLE

> Quantitative sports betting terminal — ELO, Poisson, SGM, Kelly, CLV.
> Built for NRL and AFL. Bloomberg-style dark theme. Self-hosted, no tracking.

**Live:** https://thotsl4yer69.github.io/oracle/

## What it is

ORACLE is a single-file analytics terminal for systematic sports betting. It treats betting markets the way a desk treats FX: edge measured against the closing line, sized with fractional Kelly, exposures bounded by hard risk limits.

The page is one HTML file with inline CSS and vanilla JS. No build pipeline, no framework, no telemetry. Drop it anywhere static, point a domain at it, done.

## Modules

| Module           | What it does                                                                |
| ---------------- | --------------------------------------------------------------------------- |
| **ELO**          | Power ratings for NRL and AFL. k=20/22, HFA tuned per league. 7d delta.     |
| **Poisson**      | Score simulator. Dixon-Coles correction. Win/draw/over probabilities.       |
| **SGM Builder**  | Same-game multi pricer with naive vs correlated fair odds + edge readout.   |
| **Kelly Sizer**  | Fractional Kelly with edge, EV, and risk-of-ruin estimate.                  |
| **CLV Tracker**  | 30d closing line value sparkline + per-bet table.                           |
| **Bankroll**     | Balance, available, locked, HWM, drawdown, unit size.                       |
| **Risk Limits**  | Max stake %, max correlation, daily loss cap, min edge, max SGM legs.       |
| **Signal Feed**  | Live edge, steam, injury, model, stale-line, weather signals.               |

## Models

- **ELO** — `R' = R + k(actual − expected)` with home-field added as a virtual rating bump. Expected = `1 / (1 + 10^((Ra − Rh − HFA) / 400))`.
- **Poisson scoring** — independent home/away score draws, with Dixon-Coles ρ to correct low-score correlation. Iterated over all (h,a) pairs out to 80 each to compute win/total/over distributions.
- **SGM correlation** — naive product, then apply correlation discount (positive corr → discounted true price, negative → premium). Compare against bookie's offered SGM to surface edge.
- **Kelly** — `f* = (p(o−1) − (1−p)) / (o−1)`. Half-Kelly default. Stake = `bankroll · f* · fraction`.

## Run locally

```bash
git clone https://github.com/thotsl4yer69/oracle
cd oracle
python -m http.server 8080
# open http://localhost:8080
```

Or just open `index.html` in a browser.

## Deploy

GitHub Pages from `main` branch root. No build step.

## Roadmap

- [ ] Wire live odds feed (Sportsbet, TAB, Pointsbet, Bet365 scrapers)
- [ ] Persist bet log to localStorage + IndexedDB
- [ ] Backtester for ELO+Poisson against historical seasons
- [ ] CSV import for historical results
- [ ] Player props model (try-scorer, goal-kicker, disposals)

## Disclaimer

Gamble responsibly. 18+ only. This is research software. The numbers shown on the demo page are illustrative — wire your own data before staking real money.

## License

MIT © MZ1312
