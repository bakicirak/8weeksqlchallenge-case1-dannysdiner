# Danny's Diner: Customer Behaviour & Loyalty Programme Analysis (SQL)

**Business question:** How do customers visit, spend and choose items, and is it worth expanding the loyalty programme?

**Stack:** BigQuery Standard SQL · CTEs · window functions (`RANK`, `DENSE_RANK`) · conditional aggregation
**Source:** [8 Week SQL Challenge, Case Study #1](https://8weeksqlchallenge.com/case-study-1/). This small dataset (3 customers, 15 orders) is used to show the analysis approach, not to make statistically meaningful claims.

---

## Results at a glance

| # | Question | Answer |
|---|---|---|
| 1 | Total spend per customer | A **$76** · B **$74** · C **$36** |
| 2 | Visit days per customer | A 4 · B **6** · C 2 |
| 3 | First item purchased | A: curry + sushi (same visit) · B: curry · C: ramen |
| 4 | Most purchased item | **Ramen**, 8 times |
| 5 | Favourite item per customer | A: ramen · B: tie (curry/sushi/ramen) · C: ramen |
| 6 | First item after joining | A: curry (on join day) · B: sushi |
| 7 | Last item before joining | A: curry + sushi · B: sushi |
| 8 | Items / spend before joining | A: 2 items, $25 · B: 3 items, $40 |
| 9 | Points (10/$; sushi 2x) | A 860 · B 940 · C 360 |
| 10 | Points with first-week 2x bonus (to 31 Jan) | A **1,370** · B **820** |

## Insights

- **Two different customer types.** A spends the most per visit ($19), while B visits most often (6 days, $12 per visit). A loyalty programme should reward both frequency and basket size, not only spend.
- **Ramen is the core product.** It is the most ordered item overall and the favourite of A and C. It is a good anchor for promotions. Sushi is the cheapest item but has the 2x multiplier, so the biggest rewards go to the lowest-priced item. That is worth checking against margin data.
- **Joining increases activity right away.** A bought on the day they joined. A's first-week bonus is the reason A finishes January with 1,370 points against B's 820, even though their total spend is similar.
- **C is the obvious acquisition target.** C is a non-member, loyal to ramen, with low frequency. C is a good candidate for a "join and get 2x on ramen" offer.

## Recommendations

1. Expand the programme, with **frequency-based rewards** in addition to spend-based points.
2. Check the **sushi multiplier against margin**. Moving the multiplier to ramen would align rewards with the main product.
3. Track programme KPIs after launch: **member share of revenue**, **visits per member per month**, **spend per visit before vs. after joining**, and **points liability**.

## Analytical notes

- **Ties are kept**: `RANK`/`DENSE_RANK` instead of `ROW_NUMBER`. `order_date` has no time part, so two items bought on the same day are both a correct "first item". `ROW_NUMBER` would pick one of them at random.
- **Join day counts as a member purchase** (Q6, Q10). This is a business assumption and is stated in the code.
- Q10 bonus window: join date + 6 days, inclusive. Orders after 31 Jan 2021 are excluded.

## Files

| File | Content |
|---|---|
| [`schema.sql`](schema.sql) | Schema and sample data (PostgreSQL syntax, from the challenge) |
| [`solutions.sql`](solutions.sql) | All 10 solutions (BigQuery Standard SQL) |
