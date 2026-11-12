# S&P 500 List of Stock Symbols

A plain-text list of the ticker symbols currently in the S&P 500 index, one symbol per line,
ready to read straight into a trading bot, screener, or data-download script.

**File:** `s-and-p-500-stock-symbols-list.txt`
**Symbols:** 503 (the index holds 503 common stocks issued by 500 companies — a few companies have two share classes)
**Last updated:** 2026-11-11

## Ticker format

Multi-class share symbols use a **hyphen**, not a dot:

```
BRK-B
BF-B
```

This is the Yahoo Finance / `yfinance` convention. If your broker API expects dots
(Alpaca, for example, uses `BRK.B`), convert on load:

```python
symbols = [line.strip().replace('-', '.') for line in open('s-and-p-500-stock-symbols-list.txt') if line.strip()]
```

## Usage

Python:

```python
with open('s-and-p-500-stock-symbols-list.txt') as f:
    symbols = [line.strip() for line in f if line.strip()]

print(len(symbols))   # 503
```

Shell:

```bash
wc -l s-and-p-500-stock-symbols-list.txt
grep -x NVDA s-and-p-500-stock-symbols-list.txt
```

## Ordering

Symbols are kept in the index's standard listing order (alphabetical by company name,
not by ticker), so `MMM` is first and `ZTS` is last. Sort it yourself if you need
ticker order:

```bash
sort s-and-p-500-stock-symbols-list.txt > sorted.txt
```

## Changes in this update

Index membership shifts a few times a quarter through additions, deletions, mergers,
spin-offs, and ticker changes. This revision moved the list from 493 to 503 symbols.

Notable recent additions include `APP`, `HOOD`, `CVNA`, `CRH`, `VRT`, `MRVL`, `FLEX`,
`RDDT`, `FERG`, `ILMN`, and `TWLO`, along with ticker changes and spin-off entities
such as `BNY` (BNY Mellon), `FISV` (Fiserv), `MRSH` (Marsh McLennan), `Q` (Qnity
Electronics), `FDXF` (FedEx Freight), and `HONA` (Honeywell Aerospace).

Removed names include `WBA`, `WBD`, `EA`, `K`, `CAG`, `CPB`, `JNPR`, `MMC`, `FI`,
`BK`, `PARA`, `KVUE`, and `ENPH`.

## Keeping it current

Re-pull the constituent list and rewrite the file:

```bash
curl -sS https://raw.githubusercontent.com/datasets/s-and-p-500-companies/main/data/constituents.csv \
  | tail -n +2 | cut -d, -f1 | sed 's/\./-/' > s-and-p-500-stock-symbols-list.txt
```

Cross-check against the Wikipedia
[List of S&P 500 companies](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies)
page, which tracks additions and removals as S&P announces them.

## Notes

This list is provided for convenience and may lag official S&P Dow Jones Indices
announcements. Verify membership against your data provider before trading on it.
