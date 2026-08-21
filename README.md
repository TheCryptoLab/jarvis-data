# jarvis-data

Bitcoin cycle metrics, recomputed daily and published here so a scheduled agent
can read them.

## Why this repo exists

The agent that consumes these numbers runs in a sandbox whose egress allowlist
permits `api.github.com` and almost nothing else — every price-history host
(Binance, CoinGecko, mempool.space) is blocked at the proxy. So the values are
computed on a local machine and relayed through here.

## What is in it

`cycle-gauges.json` — four cycle-position gauges with their percentile rank
over a trailing four years, plus a composite count:

| Gauge | Definition |
|---|---|
| Pi Cycle ratio | 111-day SMA ÷ (350-day SMA × 2). 1.00 is the cross. |
| Mayer Multiple | price ÷ 200-day SMA |
| 2Y MA Multiplier | price ÷ (730-day SMA × 5) |
| MVRV | market cap ÷ realised cap |

`pi-cycle.json` — the Pi Cycle levels on their own.

Sources: Bitstamp BTC/USD daily closes, and the CoinMetrics community API for
MVRV. Both free and public.

## Percentiles, not thresholds

The conventional thresholds for these gauges **failed at both 2021 peaks**:

| | 13 Apr 2021 | 8 Nov 2021 | threshold |
|---|---|---|---|
| Mayer Multiple | 1.97 | 1.48 | 2.4 |
| 2Y MA Multiplier | 0.83 | 0.52 | 1.0 |
| MVRV | 3.43 | 2.85 | 3.5 |

Peak readings decay each cycle — Mayer 3.71 (2017) → 2.82 (2021), MVRV 4.72 →
3.96 — because a trillion-dollar asset cannot stretch as far from its own
moving averages as a hundred-billion one. A fixed level silently stops firing;
a percentile rank re-bases itself.

Measured precision at those thresholds, counting a hit as within 60 days of a
real top: **Mayer 49%, MVRV 25%**.

## This is not a signal

These are fitted to three or four events. Pi Cycle has exactly one genuine
out-of-sample success (April 2021); the others have none. They are useful for
noticing that conditions are unusual, and useless for timing a decision.

Nothing here is financial advice. No warranty, no guarantee of accuracy or
freshness — the file only updates when the machine that computes it is running.
