# Undertow Backtest

Does a market maker that reads liquidation zones do better than one that does not? This project replays Perpl market history against two quoting strategies and measures it. It tests the claim made by the paper-mode bot in [Undertow](https://github.com/dhruvjnv72/undertow).

**Status: in progress, started October 2026.** There are no results yet. Nothing in this README should be read as a result.

## The question

The Undertow bot:
- quotes a buy slightly below and a sell slightly above the mark price;
- widens its spread when a large liquidation zone is near;
- pulls its quotes when that zone is large compared with the order book.

Is that better than a plain fixed-spread maker, on the same data, with the same fees?

## Planned design

**Data**
- Price history: Perpl's public candle API (1-minute candles).
- Liquidation zones over time: rebuilt from decoded position events (opens, increases, decreases, closes, liquidations), plus snapshots recorded going forward.

**Engine**
- Replays candles minute by minute.
- Runs two strategies side by side on identical data: plain fixed spread and zone-aware.
- Applies maker fees and post-only rules.
- Simulates fills from candle prices (see limits).

**Output**
- Net PnL, number of fills, maximum drawdown, inventory over time.
- How often the zone-aware strategy pulled its quotes, and what the price did next.
- A written summary that states what the data can and cannot show.

## Milestones

- [ ] Project skeleton and a data loader for Perpl candles
- [ ] Zone reconstruction from position events
- [ ] Strategy interface; fixed-spread strategy
- [ ] Zone-aware strategy (same decision rules as the Undertow bot)
- [ ] Fill model and fee model, with unit tests
- [ ] Report generator
- [ ] First run on the available history, written up honestly

## Honest limits

- Only a few days of reconstructed zone history exist today, so results can show a direction, not a proven profit.
- There is no historical order book. Fills are simulated from candle highs and lows, which is a rough model.
- A backtest does not prove future results, and this project makes no profit claim.

## License

MIT, see [LICENSE](LICENSE).
