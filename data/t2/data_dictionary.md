# T2 Real-Data Input Dictionary

Dataset: **T2 ETF Data Pack**  
Prepared on: 2026-09-25  
Status: instructor-prepared classroom data; classroom trial pending.

The instructor prepared the inputs in advance. In T2, your Agent checks the daily data, writes and runs a small calculation, and produces a comparison report. You personally verify one result. Maximum drawdowns, cumulative highs, and selected peak/trough answers are deliberately absent from these input files.

## 1. Files and fields

Both CSVs are UTF-8, comma-separated, with one header row. Missing required values must be investigated; do not fill, invent, or silently remove observations.

### `daily_prices.csv` — 7,536 daily observations

| Column | Meaning |
|---|---|
| `date` | US market session date, `YYYY-MM-DD`, interpreted in `America/New_York`. |
| `ticker` | `SPY`, `TLT`, or `GLD`. Each ticker/date pair is unique. |
| `close` | Yahoo chart response's `indicators.quote[0].close`, in USD. Retained as a provider field for inspection; do not use it for the required drawdown calculation. |
| `adjusted_close` | Yahoo chart response's `indicators.adjclose[0].adjclose`, in USD, reflecting the provider's split and dividend-distribution adjustments. Use this column for the exercise. |

Rows are grouped in SPY, TLT, GLD order, with dates ascending within each group. The common window is **2016-09-01 through 2026-08-31**, inclusive: **2,512 observations per ETF**. Weekends and full-day market closures have no rows. Early-close sessions remain included. Check row counts, date order, duplicate keys, finite positive prices, first/last dates, and equal date sets across tickers before calculating. These checks alone do not establish the authenticity of a source.

Prices retain the full numeric precision of the frozen JSON inputs. Do not round them before calculation. These are ETF price series, not a separate benchmark-index series, intraday prices, or NAV. `adjusted_close` is the provider's adjusted-price measure; do not label it an independently constructed total-return index. Do not subtract today's annual fee again.

### `fund_info.csv` — three disclosed fund records

| Column | Meaning |
|---|---|
| `ticker` | Join key to daily prices. |
| `fund_name`, `asset_class`, `exchange`, `currency` | Product identity; all three price series are in USD. |
| `expense_ratio_pct` | Disclosed annual expense ratio in percentage units: `0.0945` means `0.0945%`, not `9.45%`. Preserve disclosure precision. |
| `fee_source_url` | Issuer product page supporting the fee. |
| `fee_disclosure_date` | The available disclosure/as-of description; explicit uncertainty is retained where a fee-specific date is absent. |
| `fee_accessed_on` | Recorded fee recheck date, 2026-09-25. This is separate from the fee's effective date. |

Fee source locations and qualifications:

- **SPY:** State Street product page → Fund Information → Gross Expense Ratio. The fund-information date is 2026-09-10; a fee-specific effective date is not stated. The label is gross of waivers/reimbursements.
- **TLT:** iShares product page → Fees → Expense Ratio, as of the current prospectus. The fee panel does not date that prospectus; its note says extraordinary expenses may be excluded. Do not substitute the management-fee label for the total expense-ratio label.
- **GLD:** SPDR Gold Shares product page → Overview → Expense Ratio. The selected field does not state its effective date or identify a gross/net distinction.

Use these issuer-reported annual ratios with the qualifications above. They are current disclosure snapshots, not average fees over the ten-year price window, and exclude trading costs such as bid/ask spreads.

## 2. Calculate the required result

For each ticker, sort dates ascending and use all `adjusted_close` observations in the common window:

```text
high_t = largest adjusted_close observed from the window start through day t
drawdown_t_pct = (adjusted_close_t / high_t - 1) * 100
max_drawdown_pct = minimum drawdown_t_pct across the full window
```

The high must occur on or before the selected trough. Taking the global highest and lowest prices without checking their order is invalid. When extrema tie, select the earliest trough and its earliest corresponding peak. If no loss occurs, use the first observation for both dates and zero drawdown.

Keep full precision during calculation; display maximum drawdowns to two decimal places in the report. Values are non-positive; closer to zero means a smaller loss magnitude. Compare the displayed two-decimal results for the required observation and include any ties. Record calculated peak/trough dates in the table; retain their full-precision adjusted prices for your manual check. No target answers are provided here.

## 3. Source provenance

Historical source recorded by the preparation team: Yahoo Finance chart API, with daily data and adjusted closes enabled. Daily inputs were extracted from the retained responses recorded as downloaded on **2026-09-20**. The pack preparation date is **2026-09-25**. Internal checks and independent calculation implementations agree. Independent live confirmation of that retained history is still pending; internal consistency is not proof of source authenticity.

- [SPY historical chart endpoint](https://query1.finance.yahoo.com/v8/finance/chart/SPY?period1=1472688000&period2=1788220800&interval=1d&events=div%2Csplits&includeAdjustedClose=true)
- [TLT historical chart endpoint](https://query1.finance.yahoo.com/v8/finance/chart/TLT?period1=1472688000&period2=1788220800&interval=1d&events=div%2Csplits&includeAdjustedClose=true)
- [GLD historical chart endpoint](https://query1.finance.yahoo.com/v8/finance/chart/GLD?period1=1472688000&period2=1788220800&interval=1d&events=div%2Csplits&includeAdjustedClose=true)

The API's end timestamp is exclusive (2026-09-01); the final included session is 2026-08-31. Later downloads may differ because of source revisions or adjustment changes. The instructor retains original responses, hashes, fee evidence, reference calculations, and QA separately. Fee rechecks are recorded for 2026-09-25; the retained issuer HTML snapshots date from 2026-09-20.

## 4. Your check and optional collection practice

After the Agent calculates the report, choose one ETF. Locate its reported peak and trough in `daily_prices.csv`, read the two `adjusted_close` values yourself, and use a calculator to check `(trough / peak - 1) * 100`. Record the ticker, dates, values, your result, comparison with the report, and your own judgment. Also review the three displayed drawdowns to check the observation. A two-point arithmetic check does not prove the selected pair is the worst in the whole window: inspect the Agent's full-series method as well.

You may practise gathering your own data after the required task: record source URL, access date, units, dates, adjustment method, and permitted use; keep it under `artifacts/t2/exploration/`. Compare it with the frozen inputs and your calculated result, explaining differences. Keep the required input files unchanged. Optional collection adds no mandatory submission.

## 5. Limits and distribution

The exercise measures daily-close drawdown within a fixed historical window. It does not measure intraday losses, all-time risk, future performance, or the best investment. These teaching inputs are not investment advice.

The course owner confirmed classroom and public-fork distribution of this prepared pack on 2026-09-25. Third-party source terms still apply, and the repository's code license does not relicense source data. Check permitted use separately before uploading any optional data you collect yourself.
