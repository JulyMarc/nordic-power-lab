# Nordic Power Lab

Forecasting and analysis of the Danish power market (DK1/DK2) from open data, built to be checked: every
input is stored with the time it became known, models run live against simple rules, and every claim is
written down before its test. Failed claims stay in the record.

## Write-ups
| Topic | Question | Answer in one line |
|---|---|---|
| [Imbalance-price nowcast](nowcast.md) | Can public data forecast the imbalance − day-ahead gap 30 min to 3 h ahead? | A linear model failed. A three-part boosted model beats the best simple rule by 0.6–4.9% in the backtest; the live test decides. For a wind portfolio's close-out decision it does not beat "always close". |
| [15-minute day-ahead](mtu15.md) | What changed in Danish interconnector flows when day-ahead moved to 15-minute products? | Flow changes around the full hour fell by 10.5 pp (DK1) and 7.6 pp (DK2); schedules on every SDAC border turned quarter-hourly, the GB cable did not. |

## Live
- **[Status page](https://julymarc.github.io/nordic-power-lab/status/)** (updated every 30 minutes) shows the newest forecasts, how many have been issued and scored, and data
  freshness.
- **Model-vs-rule scores are sealed** until pre-registered dates (16 Dec 2026, 18 Dec 2026, 18 Feb 2027).

## How it runs
- **Ingestion.** Energi Data Service is polled every 10 minutes and ENTSO-E hourly, into DuckDB. Every row keeps
  its fetch time ("vintage"), so a backtest can ask "what was known at 14:02?".
- **Live forecasts.** Issued every 15 minutes, refit daily, scored daily against the published price in an
  append-only ledger.
- **Operations.** Data-quality checks alert on change; daily snapshots; weekly off-site backups; CI with
  tests on every push.
- **Methods.** Pre-registration, a trial log, deviation records, day-block bootstrap intervals with
  multiple-testing correction, and publication lags that are measured, not assumed.

## Code
The code lives in a private repository and is available on request.

## Data
Energi Data Service (Energinet), CC BY 4.0. ENTSO-E Transparency Platform. No data is taken from Nord Pool's
website.
