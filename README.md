# Fast-Food Sales Analysis — 1,000 orders, 3 items, 64.6% of revenue

One store's sales extract, analysed to answer the question a franchise owner actually asks:

> **Which items and which selling hours carry this store's revenue — and what should change?**

`Python` · `pandas` · Jupyter · 1,000 orders × 10 columns · dataset: [`data/balaji-fast-food-sales.csv`](data/balaji-fast-food-sales.csv)

---

## Headline findings

| # | Finding | Evidence |
|---|---|---|
| 1 | **Three items carry 64.6% of revenue** | Sandwich 65,820 (23.9%), Frankie 57,500 (20.9%), Cold coffee 54,440 (19.8%) — out of 275,230 |
| 2 | **Food is 68.6% of revenue, beverages 31.4%** | 188,840 from 686 food orders vs 86,390 from 314 beverage orders |
| 3 | **Night is the peak daypart** | 62,075 (22.6% of revenue), average order 303 — the highest of the five dayparts |
| 4 | **11.6% of orders have no payment method recorded** | Cash 48.3%, Online 40.2%, blank 107 orders / 31,795 |
| 5 | **Average order: 275.23 for 8.2 units** | 275,230 ÷ 1,000 orders; average unit price 33.72 |

Daypart detail: Night 62,075 · Afternoon 56,345 · Morning 53,730 · Evening 52,355 · Midnight 50,725. Revenue per daypart is almost flat, but **order value** is not: Night 303 vs Midnight 255.

## What I would do with this

1. **Protect the top three.** 64.6% of revenue sits on three menu lines. Stock, staffing and pricing decisions for Sandwich, Frankie and Cold coffee are the highest-leverage actions available — and the biggest single-point-of-failure risk.
2. **Attach a drink to food.** Beverages are already 31.4% of revenue from 31.4% of orders; a food-and-drink bundle is the cheapest revenue-per-order gain on the table.
3. **Close the payment gap.** 107 orders (11.6% of revenue) have no recorded method. That is not rounding error — it is untracked cash or an input process to fix before any reconciliation is possible.
4. **Fix dates before any time-series work.** The date column is inconsistent (below), so this analysis deliberately reports dayparts, not months, rather than publishing a trend it cannot defend.

## Data quality notes (read before reusing this dataset)

| Issue | Detail | Consequence |
|---|---|---|
| Mixed date formats | 403 rows `DD-MM-YYYY`, 597 rows `M/D/YY` | Month/year aggregation is unreliable — a naive parse invents 348 distinct "months". Normalise first. |
| Missing payment method | 107 of 1,000 rows blank | 11.6% of revenue unclassifiable by channel |
| No cost column | Revenue only, no cost of goods | Findings are about revenue, not profit or margin |
| Wide quantity range | 1–15 units per order | Bulk buyers may distort per-order averages |

## Method

1. Loaded the CSV and profiled shape, dtypes, nulls and cardinality with `pandas`.
2. Aggregated `transaction_amount` by `item_name`, `item_type`, `time_of_sale` and `transaction_type`.
3. Ranked items by revenue and computed the top-3 concentration share.
4. Computed average order value and average unit price from the same extract.
5. Translated each finding into the business action it implies.

## Files

```
notebooks/fast_food_business_analysis.ipynb   full analysis with outputs
data/balaji-fast-food-sales.csv               raw extract (1,000 rows)
```

## Run it

```bash
pip install pandas numpy matplotlib seaborn
jupyter notebook notebooks/fast_food_business_analysis.ipynb
```

## Next

Add a cost column to get margin per item — the top-3 concentration looks very different once COGS is included — and normalise dates to unlock month-over-month seasonality.

---

**Md. Abir Ul Islam** · [GitHub](https://github.com/abir2022) · [LinkedIn](https://www.linkedin.com/in/md-abir-ul-islam/)
