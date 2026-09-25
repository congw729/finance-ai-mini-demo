# Finance AI Mini Demo

This small repository is the shared project for three Finance × AI tutorials. T1 starts with a fixed synthetic ETF snapshot. T2 uses instructor-prepared daily price data: learners ask an Agent to write and run a small analysis, verify its result, and save a report without collecting history during class.

## Project Goal

Develop a clear and reproducible research workflow for comparing three familiar asset classes:

- `SPY` — US equities
- `TLT` — long-term US Treasury bonds
- `GLD` — gold

The project begins with a written plan in T1. Later tutorials can use the same repository to design a bounded analysis task and organize a verifiable agent workflow.

## T1 Repository Contents

```text
.
├── README.md
├── data/
│   ├── etf_snapshot.csv
│   └── data_dictionary.md
├── tasks/
│   └── t1_project_setup.md
└── artifacts/
    └── t1/
```

## T1 Output

During T1, JiuwenSwarm creates:

```text
artifacts/t1/project-plan.md
```

Students review the file and then save it with Git. JiuwenSwarm must not commit or push the change during T1.

## T2 Instructor-prepared Data

The instructor has prepared this data pack in advance so everyone can start with the same inputs and focus on using an Agent, checking its results, and saving their work. Data collection is not required during the core T2 exercise.

The student input is **two CSVs plus one data dictionary**:

| File | What it contains |
|---|---|
| [daily_prices.csv](data/t2/daily_prices.csv) | 7,536 daily observations for SPY, TLT, and GLD; columns `date`, `ticker`, `close`, `adjusted_close`. No precomputed drawdown or selected peak/trough answers. |
| [fund_info.csv](data/t2/fund_info.csv) | Three fund records: identity, disclosed annual expense ratio, source URL, disclosure-date description, and access date. |
| [data_dictionary.md](data/t2/data_dictionary.md) | Field definitions, units, dates, sources, calculation method, and limitations. |

The historical window is **2016-09-01 through 2026-08-31**, with 2,512 daily observations per ETF. Prices are in USD and retain source precision. Calculate maximum drawdowns from `adjusted_close`; keep their negative signs and display two decimal places. Fees are issuer disclosures recorded as rechecked on **2026-09-25**, in percentage units without a `%` sign; they do not describe ten-year average fees.

The **T2 ETF Data Pack** was prepared on **2026-09-25** from historical responses recorded as downloaded on 2026-09-20. Internal consistency and reference calculations have been checked; independent live confirmation of the retained history remains pending. T1's synthetic files remain unchanged and must not be used as T2 evidence. In your report, record the preparation date and historical window; no internal revision number is required.

In class, ask one Agent to inspect the inputs, write and run a small local calculation using an instructor-approved existing Python 3 or Node.js runtime, and produce `artifacts/t2/etf-comparison.md`. Let the program process the full CSV locally; do not paste all 7,536 rows into a model prompt. Keep the calculation script under `artifacts/t2/` for inspection and reruns; only the report is a required submission.

The report contains a three-ETF expense-ratio/drawdown table with calculated peak/trough dates, one drawdown observation, and one personally written check. Choose one ETF, look up the two reported dates and adjusted prices yourself, check `(trough / peak - 1) * 100` with a calculator, and review whether the three displayed results support the observation. Also inspect the full-series method: a two-point check alone cannot prove the worst drawdown. A second observation/check is optional.

### Optional: Collect and Check Your Own Data

After completing the required exercise, you are encouraged to practise collecting data yourself:

1. Follow the dictionary's source links to check an issuer's expense ratio, or obtain historical prices from a suitable data provider. Record the URL, access date, units, and adjustment method.
2. For a comparable drawdown, use the same dates, daily frequency, and price basis. You may ask an Agent to help with collection or calculation, then inspect the source and verify the result yourself.
3. Save your files and notes separately, for example under `artifacts/t2/exploration/`. Compare them with the frozen daily inputs and your computed results, explaining differences such as a different window, fee disclosure, or provider adjustment.

Keep the instructor's files unchanged and continue to use them for the required report and manual check. This exploration is optional and adds no required submission. Check the source's permitted use before uploading downloaded data to your public fork.

Teacher reference answers, original responses, calculations, and QA are maintained separately from this student pack. The course owner confirmed classroom and public-fork distribution of this pack on 2026-09-25. Source review and the classroom trial remain pending; follow the instructor's release and fork-update instructions before class.

## Data Notice

The values in `data/etf_snapshot.csv` are illustrative teaching inputs, not live or historical market observations. They must not be used as investment advice or as the basis for a real investment decision.

## License

The teaching materials and code are available under the Apache License 2.0. See `LICENSE`. Third-party financial data remains subject to its sources' terms; this license does not grant redistribution rights to the T2 source data or extracts.
